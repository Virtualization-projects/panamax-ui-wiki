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

With Kubernetes, service discovery is facilitated by creating a Kubernetes Service and linking it to a Pod. Once a Service is running, any subsequent pods will receive a set of environment variables that contain information about the IP address and port number that can be used to connect to the container associated with the Service.

For our sample template above, the Kubernetes Adatper will configure a single Service as follows:

    {  
      "id":"db-1",
      "apiVersion":"v1beta1",
      "kind":"Service",
      "port":3306,
      "labels":{  
        "name":"db"
      },
      "selector":{  
        "name":"db"
      },
      "containerPort":3306
    }

With this service running, each of the containers will receive the following environment variables when they are started:

    DB_1_PORT=tcp://127.0.0.1:3306
    DB_1_PORT_3306_TCP=tcp://127.0.0.1:3306
    DB_1_PORT_3306_TCP_PROTO=tcp
    DB_1_PORT_3306_TCP_PORT=3306
    DB_1_PORT_3306_TCP_ADDR=127.0.0.1

Note that these environment variables follow exactly the same conventions that Docker uses when linking containers that are running on the same host. By using these environment variables in your application for service discovery you should be able to run the same container using both Docker links and Kubernetes Services.

### Scaling

During the deployment process, the user is provided an opportunity to scale the services being deployed. For example, if a user has a cluster of 3 nodes the user might choose to scale the WordPress service across all 3 nodes. 

In this case the Kubernetes Adapter will create a Kubernetes Replication Controller for the WordPress Pod:

    {  
      "id":"wp-replication-controller",
      "apiVersion":"v1beta1",
      "kind":"ReplicationController",
      "desiredState":{  
        "replicas":3,
        "replicaSelector":{  
          "name":"wp"
        },
        "podTemplate":{  
          "desiredState":{  
            "manifest":{  
              "id":"wp-replication-controller",
              "version":"v1beta1",
              "containers":[{  
                "name":"wp",
                "image":"centurylink/wordpress:3.9.1",
                "ports":[{  
                  "hostPort":8000,
                  "containerPort":80,
                  "protocol":"TCP"
                }],
                "env":[{  
                  "name":"DB_PASSWORD",
                  "value":"pass@word01"
                }, {  
                  "name":"DB_NAME",
                  "value":"wordpress"
                }]
              }]
            }
          },
          "labels":{  
            "name":"wp"
          }
        }
      },
      "labels":{  
        "name":"wp"
      }
    }

The Replication Controller will do everything it can to ensure that exactly three instances of the WordPress service remain running at all times.

Whenever a Replication Controller is created, the Kubernetes Adapter will also create a Kubernetes Service to act as a proxy/load-balancer for the replicated Pod:

    {  
      "id":"wp",
      "apiVersion":"v1beta1",
      "kind":"Service",
      "port":8000,
      "labels":{  
        "name":"wp"
      },
      "selector":{  
        "name":"wp"
      },
      "containerPort":80
    }

With this Service in place any requests to port 8000 will be automatically load-balanced between the three WordPress pods being managed by the Replication Controller.

## Caveats

* Any volume configuration (either volumes mounted from the host or from other containers) defined in your application template will **not** translate to Kubernetes.

* Kubernetes allows only a narrow set of valid characters when naming Pods, Replication Controllers and Services. Names may only contain lowercase letters, numbers and the `-` (dash) character. In order to comply with the Kubernetes naming restrictions, the adapter will automatically alter the passed in service names -- all uppercase letters will be down-cased and any other dis-allowed characters will be substituted with a `-` (dash) character.

* In order for container links to work, you must explicitly expose any ports that the linked-to container is listening on. When using container links locally Docker has the ability to inspect the image and see any exposed ports which were defined in the Dockerfile. With Kubernetes the linked containers may be on different nodes in the cluster so the exposed ports must be explicitly defined in the application template so that the Kubernetes Service can be properly configured.

* When using container links locally Docker will inject service discovery environment variables into the parent container for *each* of the ports exposed by the child container. When using Kubernetes Services for service discovery only a single port can be specified. If the child container in the link relationship exposes more than one port (see the point above) only the lowest numbered port will be used for the Kubernetes Service configuration.