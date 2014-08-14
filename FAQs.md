# Frequently Asked Questions

##### Can I use private registries and private images to build apps in Panamax?

Although we don't search private docker registries, you can use them in Panamax. To use a private image, first enter the CoreOS host with `$ panamax ssh` and pull your image using the docker pull command. After pulling the image, you can search for your image directly in Panamax. It will show up as a 'Local' image. You can now use it to build your application and template.

##### Are the templates I create in Panamax exposed to the public? My start-up is in Stealth Mode. 

They don't have to be. If the Github access token you provided to Panamax is scoped to access your private repos, you can save them there. We don't force saving to public repositories.

##### Can I create an application in Panamax that runs across multiple hosts? 

Not yet, currently an application exists on a single host but since CoreOS/Fleet has strong support for multi-hosts, we will eventually support it as well. 

##### Can I install Panamax on Windows?

Well, yes and no. Although Panamax can possibly (read not tested by us) be installed on Windows, we don't have an installer for Windows. But, that is a great opportunity for you to contribute to Panamax. Please find the Panamax installer [source code repo](https://github.com/CenturyLinkLabs/panamax-coreos) on Github.

##### Can I use the Docker command line ? 

Yes, you can. `$ panamax ssh` will let you ssh into your coreos virtual machine. Once inside, you can run any command directly if you would like. 

We also have a web-based terminal packaged as a Docker image that you can run -- this will allow you to interact with the Docker command line right from your browser. Check-out the [centurylink/coreos-cli-wetty](https://registry.hub.docker.com/u/centurylink/coreos-cli-wetty/) image on the Docker hub for more information.

##### Can I increase the memory and CPU cores for the VM that Panamax uses?

Sure you can. To change the memory and CPU allocation for the VM you will either need to issue a `$ panamax reinstall --memory=1024 --cpu=2`, adding your own needed values, or you can stop the VM with `$ panamax pause` and manually edit the `PMX_VM_MEMORY` and `PMX_VM_CPUS` entries in the `~/.panamax/.env` file. Then restart the VM using `$ panamax up`. This second method prevents the destruction and recreation of your CoreOS VM. [Learn more about Panamax Installer commands.](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Panamax-Installer-Commands)

##### How can I easily see all of the Public Panamax Templates

You can see them in the [Panamax Public Templates repository](https://github.com/CenturyLinkLabs/panamax-public-templates) or from within Panamax by doing a search for: **all**

##### Why does my service seem to be in an infinite loop of starting/failing/stopping?

When launching an image you will sometimes see a pattern like the following in the journal output:

	Aug 13 08:15:20 systemd Starting mysql_latest.service...
	Aug 13 08:16:20 docker Pulling repository mysql
	Aug 13 08:16:33 docker Error response from daemon: No such container: mysql_latest
	Aug 13 08:16:33 docker 2014/08/13 15:16:33 Error: failed to remove one or more containers
	Aug 13 08:16:33 systemd Started mysql_latest.service.
	Aug 13 08:16:33 docker error: database is uninitialized and MYSQL_ROOT_PASSWORD not set
	Aug 13 08:16:33 docker Did you forget to add -e MYSQL_ROOT_PASSWORD=... ?
	Aug 13 08:16:33 systemd mysql_latest.service: main process exited, code=exited, status=1/FAILURE
	Aug 13 08:16:33 docker Error response from daemon: No such container: mysql_latest
	Aug 13 08:16:33 docker 2014/08/13 15:16:33 Error: failed to kill one or more containers
	Aug 13 08:16:33 systemd mysql_latest.service: control process exited, code=exited status=1
	Aug 13 08:16:33 docker Error response from daemon: No such container: mysql_latest
	Aug 13 08:16:33 docker 2014/08/13 15:16:33 Error: failed to remove one or more containers
	Aug 13 08:16:33 systemd Unit mysql_latest.service entered failed state.
	Aug 13 08:16:43 systemd mysql_latest.service holdoff time over, scheduling restart.
	Aug 13 08:16:43 systemd Stopping mysql_latest.service...
	Aug 13 08:16:43 systemd Starting mysql_latest.service...
	Aug 13 08:16:43 docker Pulling repository mysql
	Aug 13 08:16:46 docker Error response from daemon: No such container: mysql_latest
	Aug 13 08:16:46 docker 2014/08/13 15:16:46 Error: failed to remove one or more containers
	Aug 13 08:16:46 systemd Started mysql_latest.service.
	Aug 13 08:16:46 docker error: database is uninitialized and MYSQL_ROOT_PASSWORD not set
	Aug 13 08:16:46 docker Did you forget to add -e MYSQL_ROOT_PASSWORD=... ?
        . . . continues trying to start the service, fails, restarts . . .

While it can be disconcerting to see all of these errors piling up in the log output it's not entirely unexpected.

When you select an image and click the 'Run Image' button we generate a systemd unit file and send it off to Fleet. For the official *mysql* image shown above, it might look something like this:

	[Unit]

	[Service]
	ExecStartPre=-/usr/bin/docker pull mysql:latest
	ExecStart=/usr/bin/docker run --rm --name mysql_latest mysql:latest
	ExecStartPost=-/usr/bin/docker rm mysql_latest
	ExecStop=/usr/bin/docker kill mysql_latest
	ExecStopPost=-/usr/bin/docker rm mysql_latest
	Restart=always
	RestartSec=10
	TimeoutStartSec=5min

The inclusion of the `Restart=always` flag means that systemd is going to try and keep this thing running no matter what happens. In most cases, this is probably what you want -- if your service were to exit unexpectedly, systemd will restart it for you.

In the case of the *mysql* image, the Docker container was not even able to start. Note the line in the log output that says "Did you forget to add -e MYSQL_ROOT_PASSWORD=... ?". Apparently, this image has a hard requirement that you inject a value for the `MYSQL_ROOT_PASSWORD` environment variable and won't even start the service unless you do. It's a good idea to look at any documentation that may be associated with an image (or even the *Dockerfile* itself) to understand what requirements an image may have in order for it to run.

So the behavior shown in the log output above was a result of systemd attempting to start the container, the container exiting and then systemd restarting the container . . . lather, rinse, repeat. It may be a bit alarming to see so many error messages scrolling by, but it isn't actually doing any harm. After each failed start, the container is cleaned-up and then systemd tries again.

In the example above, going to the service management screen and adding the `MYSQL_ROOT_PASSWORD` environment variable probably would have allowed the container to start-up. Of course, you can't actually get to that screen until after you run the image so the container will definitely fail a few times before you're able to supply it with the appropriate configuration data.

This is one of the areas where we're definitely looking for feedback from the community. Right now, we try and run everything as soon as you select it -- another approach would be to allow the user to fully configure everything **and then** launch it.  Let us know what you think.