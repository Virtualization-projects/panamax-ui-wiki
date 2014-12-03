# Panamax Components

In an effort to organize all the Panamax components, some components have been logically grouped into a set of components (Panamax Client and Deployment Target).

## Panamax Client

The Panamax Client is the core Panamax tool, that users may run on their local development machines, which enables the assembly of multi-container applications.

The Panamax Client is comprised of several components, some are Panamax specific and some are not. They are outlined below:

### [Panamax Client Installer](https://github.com/CenturyLinkLabs/panamax-coreos)

Bootstrap an OS X machine with the components that makeup the Panamax Client.

* To be run as a bash script
* Dependencies: OS X
* Languages: Bash

### [Panamax Local Agent](https://github.com/CenturyLinkLabs/panamax-api)

This is where most of the magic happens. The Local Agent is responsible for communicating with many dependencies, including the local Docker host, which enables Panamax to assemble multi-container applications. The local agent also communicates to remote agents (see below), Docker registries, and more. An API is exposed by the Local Agent therefore any actions accomplished via the UI (see below) can be executed directly against the Local Agent.

* To be run as a Docker container
* Dependencies: Fleet, CoreOS
* Languages: Ruby
* Framework: Rails
* Exposes an API: REST API


### [Panamax User Interface](https://github.com/CenturyLinkLabs/panamax-ui) 

The User interface is where all the pointing, clicking, dragging, and dropping occur. The UI proxies through the Local Agent to perform Panamax operations. 

* To be run as a Docker container
* Dependencies: [Local Agent](https://github.com/CenturyLinkLabs/panamax-api), [Base UI kit](https://github.com/CenturyLinkLabs/ctl-base-ui)
* Languages: Ruby, Javascript, HTML, CSS
* Framework: Rails
* Exposes an API: No


### [CoreOS](https://coreos.com)

The Panamax Client is built on top of CoreOS as it's Docker client. The Panamax Local Agent leverages many features of CoreOS in addition to the standard Docker features. 

* [Fleet](https://github.com/coreos/fleet) - The Panamax Client does not always interact with Docker directly, but leverages Fleet as it's Container Orchestrator. CoreOS comes with Fleet installed and must be running for the Local Agent to function properly (the Panamax Client installer takes care of this, but in development one may need start fleet explicitly).
* [Docker](https://www.docker.com/) - For certain features, like searching local docker images, Panamax hits the Docker API directly.
* [Journalctl](http://www.freedesktop.org/software/systemd/man/systemd-journal-gatewayd.service.html) - Panamax parses and surfaces some of the journal output.

_NOTE: CoreOS is not specific to or owned/maintained by the Panamax team_


## Deployment Target

A Deployment Target facilitates the deployment of a Panamax Template. In practice this is typically accomplished via the Panamax Client. A single Panamax Client can register one or more deployment targets and a single Deployment target can be registered with multiple Panamax Clients.

### [Deployment Target Installer](https://github.com/CenturyLinkLabs/panamax-remote-agent-installer)

Bootstrap a docker host machine with the components that makeup a Panamax Deployment Target.

* To be run as a bash script
* Dependencies: OS with Docker installed
* Languages: Bash

### [Panamax Remote Agent](https://github.com/CenturyLinkLabs/panamax-remote-agent)

A small, containerized agent which runs in the remote environment (Deployment Target) and listens for requests from the Panamax Client via a REST API. These requests are coerced into instructions that get passed to an Orchestration adapter.

* To be run as a Docker container
* Dependencies: one of the Panamax Orchestration Adapters
* Languages: Ruby
* Framework: Rails
* Exposes an API: REST API


### [Orchestration Adapter](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Adapter-Developer's-Guide)

Encapsulates all of the orchestrator-specific logic in a dedicated adapter layer that is distinct from the agent. The user is able to select the adapter that matches the orchestration technologies available in their target environment. [more...](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Adapter-Developer's-Guide)

* To be run as a Docker container
* Dependencies: orchestrator the adapter talks with
* Languages: Any
* Exposes an API: REST API (must conform to the REST API outlined in the [Adapter Guide](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Adapter-Developer's-Guide)

Some exisitng adapters include:
* [Kubernetes Adapter](https://github.com/CenturyLinkLabs/panamax-kubernetes-adapter)
* [Fleet Adapter](https://github.com/CenturyLinkLabs/panamax-fleet-adapter)


## Misc. Components

### Panamax Template Repos

A template repo is simply a repository containing ```.pmx``` template files. The Panamax Client will search template repo sources registered with that specific client. The [Panamax Public Templates Repo](https://github.com/CenturyLinkLabs/panamax-public-templates) comes seeded by default in the Panamax Client.

### Docker Registries

The Panamax client can search one or more Docker registries for images. The [Docker Hub](https://hub.docker.com) comes seeded by default, but users may register any number of additional Docker registries with a Panamax Client.