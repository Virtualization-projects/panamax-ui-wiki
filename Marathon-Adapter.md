**The Marathon adapter, in combination with the Panamax remote agent, enables the deployment of a Panamax template to a Mesosphere cluster.**

## Template Translation

To understand how the Marathon Adapter translates a Panamax Template into a deployment, let's look at an example template:

    name: Wordpress with MySQL
    images:
    - name: WP
      source: centurylink/wordpress:3.9.1
      environment:
        - variable: DB_PASSWORD
          value: pass@word01
        - variable: DB_NAME
          value: wordpress
      links:
      - service: DB
        alias: DB_1
      ports:
      - host_port: 8000
        container_port: 80
    - name: DB
      source: centurylink/mysql:5.5
      environment:
        - variable: MYSQL_ROOT_PASSWORD
          value: pass@word01
      ports:
      - host_port: 3306
        container_port: 3306

### The Panamax Remote Agent
The Panamax Remote Agent will receive the template with its deployment counts and overridden environment variables and pass that configuration to the adapter.  The adapter processes this request and will translate it into deployment requests.

### Grouping
The Marathon Adapter creates a unique group name identifier and prefixes all services with this group name when they are translated into application deployments. Therefore, all template deployments are isolated from other template deployments. When all the applications within a group have been removed the group is also removed.

### Scaling
During the deployment process, the user is provided an opportunity to scale the services being deployed.  For example, if a user has a cluster of 3 nodes the user might choose to scale the WordPress service across all 3 nodes.  The user may also override environment variables defined in the template so that during deployment, the values of these keys can be specific to the target deployment environment.

A dependent service such as MySQL in the template above can only exist on a single node unless the application uses a proxy service in front of the dependency. If a dependent service is scaled to more than one, it will automatically be limited to only one instance (a singleton service).

This approach was taken so that a template could be deployed on any Mesosphere cluster without assumptions about the cluster having DNS or proxy services for service discovery and routing.

### Service discovery
Mesosphere has documented a suggested architecture for service discovery: [Mesosphere Service Discovery](http://mesosphere.com/docs/getting-started/service-discovery/)

However, the marathon adapter does not assume the cluster was configured using this architecture. The adapter manages application deployments by transitioning a deployment through three states.

#### Requirements
By analyzing the links stanza in the template request, the adapter can determine if the given service is dependent upon another service. A deployment will remain in this state until its service requirements have been met. In the template example above, the WP service is dependent upon the DB service. Therefore, it cannot be deployed until it knows where the DB service has been deployed.

This step will result in the following 2 environment variables being set on the dependent container:

**alias_SERVICE_HOST**
IP address derived from the value associated with the etcd key of the same name created when the dependency's container is created, e.g. DB_1_SERVICE_HOST=172.17.8.101

**alias_PORT**
Full URL for the dependency's container, e.g. DB_1_PORT=tcp://172.17.8.101:3306. These values correspond to the lowest exposed port binding rule. If the child container in the link relationship exposes more than one port only the lowest numbered port will be used for the Fleet unit configuration.
 
Additionally, for each port binding rule, the following 4 variables will be set:

**alias_PORT_num_protocol**
Full URL for the dependency's container, e.g. DB_1_PORT_3306_TCP=tcp://172.17.8.101:3306

**alias_PORT_num_protocol_PROTO**
Protocol (tcp or udp) for the dependency's container, e.g. DB_1_PORT_3306_TCP_PROTO=tcp

**alias_PORT_num_protocol_PORT**
Exposed port number for the dependency's container, e.g. DB_1_PORT_3306_TCP_PORT=3306

**alias_PORT_num_protocol_ADDR**
IP address for the dependency's container, e.g. DB_1_PORT_3306_TCP_ADDR=172.17.8.101

#### Deployment
The deployment state executes the commands to deploy the application into the Mesosphere cluster.

#### Post Deployment
Once a service has been deployed it advertises its location and configuration so that services which may depend upon it can complete their requirement step.

