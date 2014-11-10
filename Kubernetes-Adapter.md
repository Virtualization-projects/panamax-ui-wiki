**The Kubernetes adapter in combination with the Panamax remote agent enables the deployment of a Panamax template to a Kubernetes cluster.**

## Template Translation

To understand how the Kubernetes Adapter translates a Panamax Template into Kubernetes-specific artifacts, let's look at an example template:

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

This template describes two services and their corresponding configuration (note that some of the metadata you typically find in Panamax templates like `description` and `category` has been stripped out of this example for the sake of brevity).

For this template, there are three Kubernetes artifacts that will be created: two Pods and one Service (note that the use of the word "service" tends to get overloaded quite a bit -- whenever you see Service with a capital 'S', know that we're talking specifically about a Kubernetes Service).  There will be on pod created for each of the services described in the template. First the Pod for the WordPress service:

    {
      "id": "wp-pod",
      "kind": "Pod",
      "apiVersion": "v1beta1",
      "desiredState": {
        "manifest": {
          "id": "wp-pod",
          "version": "v1beta1",
          "containers": [{
            "name": "wp",
            "image": "centurylink/wordpress:3.9.1",
            "ports": [{
              "containerPort": 80,
              "hostPort": 8000
            }],
            "env": [{
              "name": "DB_PASSWORD", 
              "value": "pass@word01"
            }, {
              "name": "DB_NAME", 
              "value": "wordpress"
            }]
          }]
        }
      },
      "labels": {
        "name": "wp"
      }
    }

And the Pod for the MySQL service:

    {
      "id": "db-pod",
      "kind": "Pod",
      "apiVersion": "v1beta1",
      "desiredState": {
        "manifest": {
          "id": "db-pod",
          "version": "v1beta1",
          "containers": [{
            "name": "db",
            "image": "centurylink/mysql:5.5",
            "ports": [{
              "containerPort": 3306,
              "hostPort": 3306
            }],
            "env": [{
              "name": "MYSQL_ROOT_PASSWORD", 
              "value": "pass@word01"
            }]
          }]
        }
      },
      "labels": {
        "name": "db"
      }
    }


## Caveats

* Any volume configuration (either volumes mounted from the host or from other containers) defined in your application template will **not** translate to Kubernetes.

* Kubernetes allows only a narrow set of valid characters when naming pods, replication controllers and services. Names may only contain lowercase letters, numbers and the `-` (dash) character. In order to comply with the Kubernetes naming restrictions, the adapter will automatically alter the passed in service names -- all uppercase letters will be down-cased and any other dis-allowed characters will be substituted with a `-` (dash) character.

* In order for container links to work, you must explicitly expose any ports that the linked-to container is listening on. When using container links locally Docker has the ability to inspect the image and see any exposed ports which were defined in the Dockerfile. With Kubernetes the linked containers may be on different nodes in the cluster so the exposed ports must be explicitly defined in the application template so that the Kubernetes Service can be properly configured.

* When using container links locally Docker will inject service discovery environment variables into the parent container for *each* of the ports exposed by the child container. When using Kubernetes Services for service discovery only a single port can be specified. If the child container in the link relationship exposes more than one port (see the point above) only the lowest numbered port will be used for the Kubernetes Service configuration.