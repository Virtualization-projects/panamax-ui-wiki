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

## Caveats

* Any volume configuration (either volumes mounted from the host or from other containers) defined in your application template will **not** translate to Fleet.

* Fleet allows only a narrow set of valid characters when naming services. When deploying with Panamax, names may only contain lowercase letters, numbers and the `-` (dash) character. In order to comply with the Fleet naming restrictions, the adapter will automatically alter the passed in service names -- all uppercase letters will be down-cased and any other dis-allowed characters will be substituted with a `-` (dash) character.

* For scaled services, each individual service will be given a name using the following convention:
name@instance.suffix
where name is the service name, instance is the number of the service, and the suffix is '.service.'
If the template above were to include a request for 3 wordpress services, the names would be wp@1.service, wp@2.service, and wp@3.service.
If the service is a singleton, it follows the naming convention name.service. This is solely for readability and ease when adding and retrieving the service values from etcd.

* A dependent service such as MySQL in the template above can only exist on a single node unless the application uses a proxy service in front of the dependency. If a dependent service is scaled to more than one, it will automatically be limited to only one instance (a singleton service).  

* In order for container links to work, you must explicitly define each port mapping rule for the services being deployed. When using container links locally Docker has the ability to inspect the image and see any exposed ports which were defined in the Dockerfile. With Fleet the linked containers may be on different nodes in the cluster so the exposed ports must be explicitly defined in the application template so that the resulting Fleet unit can be properly configured.

* When using container links locally Docker will inject service discovery environment variables into the parent container for *each* of the ports exposed by the child container. When using Fleet and etcd for service discovery only a single port can be specified. If the child container in the link relationship exposes more than one port (see the point above) only the lowest numbered port will be used for the Fleet unit configuration.
    
