**The Kubernetes adapter in combination with the Panamax remote agent enables the deployment of a Panamax template to a Kubernetes cluster.**

## Caveats

* Any volume configuration (either volumes mounted from the host or from other containers) defined in your application template will **not** translate to Kubernetes.

* In order for container links to work, you must explicitly expose any ports that the linked-to container is listening on. When using container links locally Docker has the ability to inspect the image and see any exposed ports which were defined in the Dockerfile. With Kubernetes the linked containers may be on different nodes in the cluster so the exposed ports must be explicitly defined in the application template so that the Kubernetes Service can be properly configured.

* When using container links locally Docker will inject service discovery environment variables into the parent container for *each* of the ports exposed by the child container. When using Kubernetes Services for service discovery only a single port can be specified. If the child container in the link relationship exposes more than one port (see the point above) only the lowest numbered port will be used for the Kubernetes Service configuration.