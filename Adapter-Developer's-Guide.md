# Overview
When composing an application with Panamax all of the Docker containers are executed within a local CoreOS instance. This local sandbox is perfect for validating that all of your containers work (and work well together) but once you've got your application configured the way you want it you're probably going to want to deploy it into an environment other than your local machine.

Panamax provides a remote deployment feature which allows the user to take an application template and send it to a remote environment for execution. Remote deployments are handled by a small, containerized agent which runs in the remote environment and listens for requests from the Panamax Client. 

Unlike local applications where having all containers on a single host is usually sufficient, remote deployments will typically involve a cluster of machines and some sort of orchestrator for deploying containers across that cluster. There are a nubmer of such orchestrators:

* [Fleet](https://coreos.com/using-coreos/clustering/)
* [Kubernetes](https://github.com/googlecloudplatform/kubernetes#kubernetes)
* [geard](http://www.openshift.org/geard/)
* [Mesos](http://mesos.apache.org/documentation/latest/docker-containerizer/)

Instead of focusing on a single Docker orchestrator, the Panamax agent was designed so that it could interact with a variety of orchestration technologies. This is accomplished by encapsulating all of the orchestrator-specific logic in a dedicated adapter layer that is distinct from the agent. The user is able to select the adapter that matches the orchestration technologies available in their target environment.

If an adapter doesn't yet exist for a particular clustering/orchestration technology it should be fairly easy for a developer to create an adapter that will meet their needs. The rest of this guide discusses the requirements for implementing an adapter that can work with the Panamax deployment agent.


# Architecture
The Panamax Client doesn't communicate directly with the adapter, instead requests for remote deployment are sent the Panamax Agent. The Agent is responsible for securing the connection between the Client and the remote environment and maintaining state about deployed applications. 

At start-up time, the Panamax Agent is associated with a single adapter using a [Docker link](https://docs.docker.com/userguide/dockerlinks/#docker-container-linking). When the Agent receives a request to deploy/delete an application it will pass it along to the configured adapter for execution.

Adapters are simply containerized applications that expose a specific HTTP REST service interface (described in detail below). As long as the adapter adheres to the interface specification, the service can be implemented with any language/framework the developer choses. The only other requirement is that the adapter's Docker image must expose the port on which the service is listening (typically done via an `EXPOSE` instruction in the Dockerfile) -- this is how the Agent will discover the adapter endpoint when the two containers are linked.

# API
The adapter API is an HTTP REST service that accepts and returns JSON-encoded entities.

## Entities
The primary entity used in the adapter API is the Service. The Service is a representation of the Docker images (along with their configuration data) which comprise an application.

### Service

* *id*: (read-only) unique identifier for service
* *name*: (required) friendly name for service (maybe used as the ID in some implementations)
* *source*: (required) Docker image to be executed
* *command*: command to be executed when container is started
* *links*: list of Link entities
* *ports*: list of Port entities
* *expose*: list of ports that should be exposed when the container is started
* *environment*: list of Environment entities
* *volumes*: list of Volume entities
* *desiredState*: state the user wishes the service to be in ("started", "stopped")
* *currentState*: (read-only) state the service is currently in

### Link
* *name*: (required) name of service to link to
* *alias*: (required) alias to assign to linked-to service

###Port
* *hostPort*: the host port which maps to the container port
* *containerPort*: (required) the port on which the container is listening
* *protocol*: The protocol for this port ("TCP", "UDP")

### Environment
* *variable*: (required) name of the environment variable
* *value*: (required) value of of the environment variable

### Volume
* *hostPath*: The host path to be mapped into the container
* *containerPath*: (required) The path at which to mount the volume inside the container

## Endpoints
### Create Services
    POST /v1/services HTTP/1.1

Executes the list of posted Service entities and returns the IDs of all the created artifacts. All of the services in an application are purposefully posted together so that the adapter has the opportunity to resolve dependencies between services and ensure that everything is started in the correct order.

The adapter doesn't necessarily need to wait until all the services have started before returning the response. The only important bit is that the response must contain the IDs of all the artifacts that are created in the process of launching these services. These IDs are cached in the Panamanx Agent and used when subsequent requests are received to retrieve the status of an application or delete an application.
    
**Example Request:**

	POST /v1/services HTTP/1.1
	Content-Type: application/json
	 
	[
	  {
	    "name": "db",
	    "source": "mysql:latest"
	  },
	  {
	    "name": "wp",
	    "source": "wordpress:latest"
	  }
	]

**Example Response:**

	HTTP/1.1 201 OK
	Content-Type: application/json
	 
	[
	  {
	    "id: "db.service",
	    "currentState": "started"
	  },
	  {
	    "id": "wp.service",
	    "currentState": "started"
	  }
	]
	
Returned Services may be partial entities containing just the service IDs or may echo back the full service entity (including the ID).

**Status Codes:**	

* **201** - all services created without error
* **409** - one or more services could not be created due to conflict with already running services
* **500** - server error

### Get Service
    GET /v1/services/(id) HTTP/1.1
Returns the status of a previously created service.

**Example Request:**

    GET /v1/services/db.service HTTP/1.1

**Example Response:**

	HTTP/1.1 200 OK
	Content-Type: application/json
	 
	{
	  "id": "db.service",
	  "currentState": "started"
	}
	
A successful response returns a partial Service entity containing the ID and the current state of the specified service. 

**Status Codes:**

* **200** - no error
* **404** - specified service cannot be found
* **500** - server error

### Destroy Service
    DELETE /v1/services/(id) HTTP/1.1
Stops and removes a previously created service.

**Example Request:**
    
    DELETE /v1/services/db.service HTTP/1.1

**Example Response:**

	HTTP/1.1 204 OK
	
A successful response returns no content.

**Status Codes:**

* **204** - no error
* **404** - specified service cannot be found
* **500** - server error

### Metadata
    GET /v1/metadata HTTP/1.1
Returns metadata about the adapter itself. At this time, the only values in the response are `version` and `type`. The `version` field should provide the version number for the adapter while the `type` field should provide some indication of the orchestrator that the adapter interacts with.

**Example Request:**

    GET /v1/metadata HTTP/1.1
    
**Example Response:**

	HTTP/1.1 200 OK
	Content-Type: application/json
	 
	{
	  "version": "1.0.1",
	  "type": "fleet"
	}
	
* **200** - no error
* **500** - server error

# Example Adapters
The Panamax team has already implemented two orchestration adapters:

* [panamax-fleet-adapter](https://github.com/CenturyLinkLabs/panamax-fleet-adapter) - Deploys applications to a CoreOS cluster using Fleet
* [panamax-kubernetes-adapter](https://github.com/CenturyLinkLabs/panamax-kubernetes-adapter) - Deploys applications using the Kubernetes cluster manager

Ideas for additional adapters include:

* Plain ol' Docker - Forget the fancy orchestrators, allow remote deployment of applications to any system running Docker
* OpenShift / geard - Deploy applications via geard to an OpenShift cluster
* Apache Mesos - Deploy applications to a Mesos cluster