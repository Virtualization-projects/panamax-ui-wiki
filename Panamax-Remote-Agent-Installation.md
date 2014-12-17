_Short URL:_ [http://j.mp/pmx_installagent](http://j.mp/pmx_installagent)

Installing the Panamax Remote Agent allows you to deploy your application directly from your Panamax client to a deployment endpoint. The following describes the process for preparing your deployment environment and how to install the Panamax Remote Agent and adapter for your specific orchestrator.

## Setup your deployment environment
Panamax allows you to deploy your application to any node or cluster of nodes utilizing our remote agent. The first step is to setup your deployment endpoint on the cloud provider of your choice with the orchestrator of your choice. The following links should provide some guidance on how to setup an environment with specific orchestrators that Panamax currently supports:

###Kubernetes
[Kubernetes Project Home](https://github.com/GoogleCloudPlatform/kubernetes/) - lots of guides for setting up Kubernetes on different environments

[Getting started on Google Compute Engine](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/gce.md)

[Google Container Engine](https://cloud.google.com/container-engine/)

[How To Install and Configure Kubernetes on top of a CoreOS Cluster](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-kubernetes-on-top-of-a-coreos-cluster)

[Single-node CoreOS Setup](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/coreos_cloud_config.md)

[Multi-node CoreOS Cluster Setup](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/coreos_cloud_config.md)

[Debugging FAQ](https://github.com/GoogleCloudPlatform/kubernetes/wiki/Debugging-FAQ)

###Fleet/etcd
[Installing CoreOS On CenturyLink Cloud](http://www.centurylinklabs.com/tutorials/openstack/installing-coreos/)

[Building CoreOS Server Cluster on the CenturyLink Cloud](https://t3n.zendesk.com/entries/47064274-Building-CoreOS-Server-Cluster-on-the-CenturyLink-Cloud)

[Running CoreOS on Google Compute Engine](https://coreos.com/docs/running-coreos/cloud-providers/google-compute-engine/)

[Running CoreOS on EC2](https://coreos.com/docs/running-coreos/cloud-providers/ec2/)

## Adding a Panamax Remote Agent node to your environment
In order to install and utilize the Panamax Remote Agent, Panamax uses a dedicated installer node. The Panamax remote agent can run on Linux with docker installed. However, we recommend using CoreOS for simplicity. 

Following the normal methods of adding a CoreOS VM with your cloud provider. Ensure that the node resides on the same VPN as your deployment node or cluster and the node has a publicly accessible IP.

**Ensure tcp port 3001 is open on your Panamax Remote Agent node.**

To test connectivity to cluster, run:

Kubernetes:

``$ telnet <kubernetes_master_IP> 443`` - connection should be successful

Fleet:

``$ telnet <public_exposed_fleet_node_IP> 4001`` - connection should be successful

## Installing the Remote Agent
Docker must be [installed](https://docs.docker.com/installation/#installation) on the node. This step is not necessary if using CoreOS as the Remote Agent. Use the following steps to install the Panamax Remote Agent:

1. SSH into the Panamax remote agent node created above
2. Run ``$ sudo su`` to ensure your docker commands will run with correct privileges
3. Run ``$ sudo bash -c "$(curl http://download.panamax.io/agent/pmx-agent-install)"`` 
4. Execute ``$ cd /root/pmx-agent && ./pmx-agent`` to invoke agent setup script.
4. Select (1) - `init`, to begin the installation process
5. Follow the prompts, including choosing your adapter, adding your API endpoint (typically a private IP) and Panamax Remote Agent endpoint (typically a public IP)
6. Copy the displayed token (needed when adding the deployment endpoint to the client)

**_Notes for Kubernetes Adapter_:**
 
- The API endpoint url (kubernetes-master node) should include `https://` and no port. For example: `https://10.x.x.x`. This is typically a private IP.
- The default API username is `admin` and your password is located in `$ ~/.kubernetes_auth`.

**_Notes for Fleet Adapter_:**
 
- The API endpoint url, which can be of any Fleet enabled node running in the cluster, should include `http://` and port 4001. For example: `http://10.x.x.x:4001`. This is typically a private IP.

**_Notes for running on Google Container Engine_:**

- The API endpoint url (k8s-containercluster-master) should include `https://` and no port. For example: `https://10.x.x.x`. This is typically a private IP.
- The default API username is `admin` and your password can be discovered by running the following commands where you have gcloud installed:

     `$ gcloud components update preview`

     `$ gcloud preview container clusters list -z _ZONE_`

You can run ``$ ./pmx-agent`` to reinstall, upgrade and run other options for the remote agent. The folder is located under root home. Access the folder, following these steps:

1. ``$ sudo su``
2. ``$ cd ~/pmx-agent``
3. ``$ ./pmx-agent``

* Learn more about the [Fleet Adapter](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Fleet-Adapter)
* Learn more about the [Kubernetes Adapter](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Kubernetes-Adapter)

## Adding a deployment endpoint to the client
With you copied token, browse to the Dashboard on the Panamax client and select Remote Deployment Targets:

![image](http://panamax.ca.tier3.io/panamax_ui_wiki_screens/Remote_target-dashboard.png)

Click on _Add a new Remote Deployment Target_, enter a friendly name and your copied token from the Remote Agent:

![image](http://panamax.ca.tier3.io/panamax_ui_wiki_screens/Add_target.png)

Click _Create Remote Deployment Target_ to save. The deploy target is now available to deploy applications.

## Helpful Links
[Fleet Adapter Troubleshooting](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Fleet-Adapter-Troubleshooting)

[Kubernetes Adapter Troublshooting](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Kubernetes-Adapter-Troubleshooting)