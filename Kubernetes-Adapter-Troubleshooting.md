# Troubleshooting the Panamax Remote Agent and Adapters
The following are some tips to help debug any issues encountered with the Panamax Remote Agent and Adapters. You will also find tips on how to verify your running applications post-deployment.

## Be Adapter Aware
If you encounter an error on the GUI, its not always obvious what the underlying issue it. We are working to bubble up more useful error messages and warnings, but for the time being, here are some tips.

First off, check out the caveats for each supported adapter and orchestrator. Consideration needs to be taken when creating and launching a template in Panamax as to which Adapter you will ultimately be deploying to. Certain behaviors and rules apply that will affect your base template creation, including image configuration and docker options.

[Kubernetes Caveats](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Kubernetes-Adapter#caveats)

## Use the Docker Logs
Currently, errors can appear on the GUI without much explanation of the underlying issue/error. Viewing the docker logs on the PMX-Agent and PMX-Adapter containers can provide some insight into the problem. Here are the steps to view these:

1. SSH into your Remote Agent node (the node you have installed the remote agent and adapter on)
2. Switch to root: `$ sudo su`
3. Run: `$ docker logs pmx-agent` to output the logs of the PMX Agent
4. Run: `$ docker logs pmx-adapter` to output the logs of the PMX Adapter

## Verify your Deployment
The Panamax client GUI provides statuses for your running services, passing the "raw" status messages from your orchestrator in the deployment detail page. You can also view these and other useful information in the orchestrator directly.

### Kubernetes
To manually check your service statuses and other orchestrator specific data in Kubernetes, you need the [kubecfg CLI](https://github.com/GoogleCloudPlatform/kubernetes/releases) installed. This can be on your local machine or a node residing in your cluster. Ensure to run all commands from the directory the source is installed. 

1. cd into the directory that contains the Kubernetes files. ex: `$ cd ~/kubernetes`
2. To list your minions: `$ cluster/kubecfg.sh -insecure_skip_tls_verify=true list minions`
   
    ```
    Minion identifier
    ----------
    kubernetes-minion-1.c.project_name.internal
    kubernetes-minion-2.c.project_name.internal
    kubernetes-minion-3.c.project_name.internal
    kubernetes-minion-4.c.project_name.internal
    ```

3. To list your pods: `$ cluster/kubecfg.sh -insecure_skip_tls_verify=true list pods`

```
ID                                     Image(s)                                 Host                                                              Labels                                                    Status
----------                             ----------                               ----------                                                        ----------                                                ----------
db-pod                                 centurylink/mysql:5.5                    kubernetes-minion-2.c.project_name.internal/162.222.xxx.xxx   name=db                                                   Waiting
4c99324e-6e91-11e4-9ea9-42010af00e3a   centurylink/wordpress:3.9.1              kubernetes-minion-4.c.project_name.internal/146.148.xxx.xxx    name=wp,replicationController=wp-replication-controller   Running
4c99a2ce-6e91-11e4-9ea9-42010af00e3a   centurylink/wordpress:3.9.1              kubernetes-minion-1.c.project_name.internal/146.148.xxx.xxx     name=wp,replicationController=wp-replication-controller   Running
4c99b773-6e91-11e4-9ea9-42010af00e3a   centurylink/wordpress:3.9.1              kubernetes-minion-3.c.project_name.internal/23.236.xxx.xxx     name=wp,replicationController=wp-replication-controller   Running

```

## Accessing your Running Applications
After you have deployed your application using Panamax, you, of course, will want to access it. How you do this, depends on your orchestrator.

###Accessing with Kubernetes
If you have launched multiple instances of your service as in the example above, you would access your app through the kubernetes proxy minion. With Kubernetes, that is always minion 1.

So from our example, you would browse to http://146.148.xxx.xxx:8080

_NOTE:_ Make sure you have this node in your cluster available to the internet, including the correct port. The port should match the container to host port binding found in your Panamax template.

If you have a single instance on your services, i.e. you did not increase the deploy count when deploying from Panamax, you can access your application using the IP address listed against the specific service pod. For example:

`4c99b773-6e91-11e4-9ea9-42010af00e3a   centurylink/wordpress:3.9.1              kubernetes-minion-3.c.project_name.internal/23.236.xxx.xxx`

In this case, you would browse to http://23.236.xxx.xxx:8080 to access your Wordpress app.

_NOTE:_ Make sure you have this node in your cluster available to the internet, including the correct port. The port should match the container to host port binding found in your Panamax template.
