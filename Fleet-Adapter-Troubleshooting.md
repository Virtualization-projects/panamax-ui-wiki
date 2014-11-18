The following are some tips to help debug any issues encountered with the Panamax Remote Agent and Adapters. You will also find tips on how to verify your running applications post-deployment.

## Be Adapter Aware
If you encounter an error on the GUI, its not always obvious what the underlying issue it. We are working to bubble up more useful error messages and warnings, but for the time being, here are some tips.

First off, check out the caveats for each supported adapter and orchestrator. Consideration needs to be taken when creating and launching a template in Panamax as to which Adapter you will ultimately be deploying to. Certain behaviors and rules apply that will affect your base template creation, including image configuration and docker options.

[Fleet Caveats](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Fleet-Adapter#caveats)

## Use the Docker Logs
Currently, errors can appear on the GUI without much explanation of the underlying issue/error. Viewing the docker logs on the PMX-Agent and PMX-Adapter containers can provide some insight into the problem. Here are the steps to view these:

1. SSH into your Remote Agent node (the node you have installed the remote agent and adapter on)
2. Switch to root: `$ sudo su`
3. Run: `$ docker logs pmx-agent` to output the logs of the PMX Agent
4. Run: `$ docker logs pmx-adapter` to output the logs of the PMX Adapter

## Verify your Deployment
The Panamax client GUI provides statuses for your running services, passing the "raw" status messages from your orchestrator in the deployment detail page. You can also view these and other useful information in the orchestrator directly.

### Fleet/CoreOS
To manually check your service statuses and other orchestrator specific data, following these steps:

1. SSH into one of your CoreOS nodes in your cluster
2. Run: `$ fleetctl list-units`
3. You should see similar output:

```
UNIT			MACHINE				ACTIVE	SUB
drupal-7-28@1.service	b457792b.../10.240.xxx.xxx	active	running
drupal-7-28@2.service	61eaf19d.../10.240.xxx.xxx	active	running
drupal-7-28@3.service	3f38918e.../10.240.xxx.xxx	active	running
mysql.service		    3f38918e.../10.240.xxx.xxx	active	running
```

###Accessing your application with Fleet
Fleet does not have a built in proxy, so you can access each node using the IP assigned for the service. A separate proxy, like [centurylink/haproxy-etcd](https://registry.hub.docker.com/u/centurylink/haproxy-etcd/) is required for load balancing. Since the `fleetctl list-units' command shows private IPs, you need to find the publicly exposed IP for that node. You can typically find this info in your cloud providers console.

_NOTE:_ Make sure you have this node in your cluster available to the internet, including the correct port. The port should match the container to host port binding found in your Panamax template