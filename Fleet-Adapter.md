**The Fleet adapter in combination with the Panamax remote agent enables the deployment of a Panamax template to a Fleet-controlled cluster.**

## Template Translation

To understand how the Fleet Adapter translates a Panamax Template into Fleet-specific artifacts, let's look at an example template:

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

This template describes two services and their corresponding configuration (note that some of the metadata you typically find in Panamax templates like `description` and `category` has been stripped out of this example for the sake of brevity)

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
Full URL for the dependency's container, e.g. DB_1_PORT=tcp://172.17.8.101:3306. These values correspond to the lowest exposed port binding rule.
 
Additionally, for each port binding rule, the following 4 variables will be set:

**alias_PORT_num_protocol**
Full URL for the dependency's container, e.g. DB_1_PORT_3306_TCP=tcp://172.17.8.101:3306

**alias_PORT_num_protocol_PROTO**
Protocol (tcp or udp) for the dependency's container, e.g. DB_1_PORT_3306_TCP_PROTO=tcp

**alias_PORT_num_protocol_PORT**
Exposed port number for the dependency's container, e.g. DB_1_PORT_3306_TCP_PORT=3306

**alias_PORT_num_protocol_ADDR**
IP address for the dependency's container, e.g. DB_1_PORT_3306_TCP_ADDR=172.17.8.101

###  Conflicting services and boot order
The scaled instances of a service cannot be co-hosted on a node, so in order to prevent this, the Fleet units include instructions to indicate service instances will conflict with other instances of the same service.  Fleet and systemd will handle the distribution of services across the cluster.

There is no guaranteed order for startup of services with Fleet.  This can pose a problem for linked services like those in the template example above.  To help in these cases, though, the Fleet adapter will write instruction to the unit file to indicate that the dependent service "wants" its dependency.  

In the end, a Fleet unit will look something like this for the MySQL service defined above.

```
[Service]
EnvironmentFile=/etc/environment
ExecStartPre=/usr/bin/etcdctl set app/MYSQL/MYSQL_SERVICE_HOST ${COREOS_PRIVATE_IPV4}
ExecStartPre=-/usr/bin/docker pull centurylink/mysql:5.5
ExecStart=-/bin/bash -c "/usr/bin/docker run --rm --name mysql -p 3306:3306 -e 'MYSQL_ROOT_PASSWORD=pass@word01' centurylink/mysql:5.5"
ExecStartPost=-/usr/bin/docker rm mysql
ExecStop=-/bin/bash -c "/usr/bin/etcdctl rm app/MYSQL --recursive && /usr/bin/docker kill mysql"
ExecStopPost=-/usr/bin/docker rm mysql
Restart=always
RestartSec=10
TimeoutStartSec=5min

[X-Fleet]
Conflicts=mysql.service
```

And something like this for one of the associated WordPress service instances.

```
[Unit]
After=mysql.service
Wants=mysql.service

[Service]
EnvironmentFile=/etc/environment
ExecStartPre=/usr/bin/etcdctl set app/WP@1/WP@1_SERVICE_HOST ${COREOS_PRIVATE_IPV4}
ExecStartPre=-/usr/bin/docker pull centurylink/wordpress:3.9.1
ExecStart=-/bin/bash -c "/usr/bin/docker run --rm --name wp -p 8000:80 -e 'DB_PASSWORD=pass@word01' -e 'DB_NAME=wordpress' -e DB_1_SERVICE_HOST=`/usr/bin/etcdctl get app/MYSQL/MYSQL_SERVICE_HOST` -e DB_1_PORT=tcp://`/usr/bin/etcdctl get app/MYSQL/MYSQL_SERVICE_HOST`:3306 -e DB_1_PORT_3306_TCP=tcp://`/usr/bin/etcdctl get app/MYSQL/MYSQL_SERVICE_HOST`:3306 -e DB_1_PORT_3306_TCP_PROTO=tcp -e DB_1_PORT_3306_TCP_PORT=3306 -e DB_1_PORT_3306_TCP_ADDR=`/usr/bin/etcdctl get app/MYSQL/MYSQL_SERVICE_HOST` centurylink/wordpress:3.9.1"
ExecStartPost=-/usr/bin/docker rm wp
ExecStop=-/bin/bash -c "/usr/bin/etcdctl rm app/WP@1 --recursive && /usr/bin/docker kill wp"
ExecStopPost=-/usr/bin/docker rm wp
Restart=always
RestartSec=10
TimeoutStartSec=5min

[X-Fleet]
Conflicts=wp@*.service
```

## Caveats

* Any volume configuration (either volumes mounted from the host or from other containers) defined in your application template will **not** translate to Fleet.

* Fleet allows only a narrow set of valid characters when naming services. When deploying with Panamax, names may only contain lowercase letters, numbers and the `-` (dash) character. In order to comply with the Fleet naming restrictions, the adapter will automatically alter the passed in service names -- all uppercase letters will be down-cased and any other dis-allowed characters will be substituted with a `-` (dash) character.

* For scaled services, each individual service will be given a name using the following convention:
`name@instance.suffix` where name is the service name, instance is the number of the service, and the suffix is '.service.'  If the service is a singleton, it follows the naming convention ```name.service```.

* A dependent service can only exist on a single node. If a dependent service is scaled to more than one, it will automatically be limited to only one instance (a singleton service).  

* In order for container links to work, you must explicitly define each port mapping rule for the services being deployed. When using container links locally Docker has the ability to inspect the image and see any exposed ports which were defined in the Dockerfile. With Fleet the linked containers may be on different nodes in the cluster so the exposed ports must be explicitly defined in the application template so that the resulting Fleet unit can be properly configured.

* When using container links locally Docker will inject service discovery environment variables into the parent container for *each* of the ports exposed by the child container. When using Fleet and etcd for service discovery only a single port can be specified. If the child container in the link relationship exposes more than one port (see the point above) only the lowest numbered port will be used for the Fleet unit configuration.
    
