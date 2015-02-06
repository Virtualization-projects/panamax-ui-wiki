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
The Panamax Remote Agent will receive the template with its deployment counts and overridden environment variables and pass that configuration to the adapter.  The adapter processes this request and will translate it into requests for Fleet units.

### Grouping
The Marathon Adapter creates a unique group name identifier and prefixes all services with this group name when they are translated into application deployments. Therefore, all template deployments are isolated from other template deployments. When all the applications within a group have been removed the group is also removed.

### Scaling
During the deployment process, the user is provided an opportunity to scale the services being deployed.  For example, if a user has a cluster of 3 nodes the user might choose to scale the WordPress service across all 3 nodes.  The user may also override environment variables defined in the template so that during deployment, the values of these keys can be specific to the target deployment environment.

A dependent service such as MySQL in the template above can only exist on a single node unless the application uses a proxy service in front of the dependency. If a dependent service is scaled to more than one, it will automatically be limited to only one instance (a singleton service).

This approach was taken so that a template could be deployed on any Mesosphere cluster without assumptions bout the cluster having DNS or proxy services for service discovery and routing.

[Mesosphere Service Discovery](http://mesosphere.com/docs/getting-started/service-discovery/)

### Service discovery
TODO: Detail Discovery states

Then, each container specified as a link will result in the following 2 environment variables being set on the dependent container:

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

### The Panamax Remote Agent
The Panamax Remote Agent will receive the template with its deployment counts and overridden environment variables and pass that configuration to the adapter.  The adapter processes this request and will translate it into requests for Fleet units.

### Scaling
During the deployment process, the user is provided an opportunity to scale the services being deployed.  For example, if a user has a cluster of 3 nodes the user might choose to scale the WordPress service across all 3 nodes.  The user may also override environment variables defined in the template so that during deployment, the values of these keys can be specific to the target deployment environment.

A dependent service such as MySQL in the template above can only exist on a single node unless the application uses a proxy service in front of the dependency. If a dependent service is scaled to more than one, it will automatically be limited to only one instance (a singleton service).

For scaled services, each individual service will be given a name using the following convention:
`name@instance.suffix` where name is the service name, instance is the number of the service, and the suffix is '.service.'

If the template above were to include a request for 3 WordPress service instances, the names would be wp@1.service, wp@2.service, and wp@3.service.

If the service is a singleton, it follows the naming convention ```name.service```.  This is solely for readability and ease when adding and retrieving the service values from etcd.

If the template above were to include a request for 3 MySQL instances, a single instance would be deployed as db.service because it is a dependency of (i.e. linked to from) the WordPress service.

### Service discovery
Fleet has no concept of service discovery, so in order to allow containers to still communicate with other containers, each container will register itself with etcd by setting an environment key ```name_SERVICE_HOST``` with the IP address of the node to which the service has been deployed.  Then, each container specified as a link will result in the following 2 environment variables being set on the dependent container:

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

###  Boot order