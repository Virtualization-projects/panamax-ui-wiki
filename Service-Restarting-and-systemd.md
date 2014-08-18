When launching an image you will sometimes see a pattern like the following in the journal output:

	systemd Starting mysql_latest.service...
	docker Pulling repository mysql
	docker Error response from daemon: No such container: mysql_latest
	docker 2014/08/13 15:16:33 Error: failed to remove one or more containers
	systemd Started mysql_latest.service.
	docker error: database is uninitialized and MYSQL_ROOT_PASSWORD not set
	docker Did you forget to add -e MYSQL_ROOT_PASSWORD=... ?
	systemd mysql_latest.service: main process exited, code=exited, status=1/FAILURE
	docker Error response from daemon: No such container: mysql_latest
	docker 2014/08/13 15:16:33 Error: failed to kill one or more containers
	systemd mysql_latest.service: control process exited, code=exited status=1
	docker Error response from daemon: No such container: mysql_latest
	docker 2014/08/13 15:16:33 Error: failed to remove one or more containers
	systemd Unit mysql_latest.service entered failed state.
	systemd mysql_latest.service holdoff time over, scheduling restart.
	systemd Stopping mysql_latest.service...
	systemd Starting mysql_latest.service...
	docker Pulling repository mysql
	docker Error response from daemon: No such container: mysql_latest
	docker 2014/08/13 15:16:46 Error: failed to remove one or more containers
	systemd Started mysql_latest.service.
	docker error: database is uninitialized and MYSQL_ROOT_PASSWORD not set
	docker Did you forget to add -e MYSQL_ROOT_PASSWORD=... ?
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