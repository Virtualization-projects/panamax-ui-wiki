### Data Volumes
Panamax currently supports the volume (-v) command for mounting a host directory as a data volume and adding a data volume to a container. Adding a data volume to a container only requires a container path.

![data volume](http://panamax.ca.tier3.io/wiki_volumes/container.png)

The example above will create a volume inside the container at /my_path and the associated docker -v command will be displayed in the dynamic docker command line at the bottom of the screen.

![data volume](http://panamax.ca.tier3.io/wiki_volumes/host-container.png)

For mounting a host directory, provide the local host path and the mapped container path. This will mount the host /var/local directory into the container at location /var/container. The proper docker -v command will be displayed in the Docker Run Command element.

### Data Volume Containers
Panamax supports the volumes from (--volumes-from) command to allow sharing of persistent data between containers. Clicking 'Mount a new Data Volume Container' will display a select box which is populated by all the other images within the current application. Selecting an image and clicking save will add the selected image as a data volume container. If the image provides any volumes, they will be displayed adjacent to the name.

![data volume](http://panamax.ca.tier3.io/wiki_volumes/volumes_from.png)

The example above shows the centurylink_data-container has already been selected and it provides two volumes: /foo/bar and /this/path/is/longer/than/the/other