### Data Volumes
Panamax supports the [volume (-v)](http://docs.docker.com/userguide/dockervolumes/#data-volumes) command for mounting a host directory as a data volume and adding a data volume to a container. Adding a data volume to a container only requires a container path.

![data volume](http://panamax.ca.tier3.io/wiki_volumes/container.png)

The example above will create a volume inside the container at /my_path and the associated docker -v command will be displayed in the docker run command displayed at the bottom of the screen.

![data volume](http://panamax.ca.tier3.io/wiki_volumes/host-container.png)

For mounting a host directory, provide the local host path and the mapped container path. This will mount the host /var/local directory into the container at location /var/container. The proper docker `-v` command will be displayed in the docker run command element.

 _NOTE_: Within Panamax, the host is always the CoreOS instance, whether running in VirtualBox, a cloud provider or bare metal.

### Data Volume Containers
Panamax also supports the [volumes from (--volumes-from)](http://docs.docker.com/userguide/dockervolumes/#creating-and-mounting-a-data-volume-container) command to allow sharing of persistent data between containers. To connect to a data volume container, click the 'Mount a new Data Volume Container' to display a select box which is populated by all the other services within the current application. Selecting an service and clicking save will add the selected service as a data volume container. If the service has any data volumes, they will be displayed adjacent to the name.

![data volume](http://panamax.ca.tier3.io/wiki_volumes/volumes_from.png)

The example above shows the centurylink_data-container service has been selected and it provides two volumes: /foo/bar and /this/path/is/longer/than/the/other