_Short URL: [http://j.mp/pmx_install](http://j.mp/pmx_install)_

We recommend the Panamax Client (UI/API) is installed locally but it can theoretically be installed anywhere [CoreOS can run.](http://coreos.com/docs/) The  instructions on this page represent known tested installations. 

[![Installing Panamax Video](http://img.youtube.com/vi/15IKkYCfymk/0.jpg)](http://www.youtube.com/watch?v=15IKkYCfymk) 
***
# Panamax Client - Local Installation (Recommended) 
Panamax is currently in BETA

**Local Client Installation Requirements**

[VirtualBox 4.3](https://www.virtualbox.org/wiki/Downloads/) or higher

[Vagrant 1.6](http://www.vagrantup.com/downloads.html/) or higher

The Panamax installer creates a VM in VirtualBox called _panamax-vm_. This VM is built on [http://coreos.com/docs/running-coreos/platforms/vagrant/](CoreOS.)

**NOTE**: The _panamax-vm_ runs with 2 CPUs and 1GB of RAM by default. These may need to be increased based on the number of containers you have running on your VM or the resources they consume. Templates may specify system recommendations for proper performance in their documentation. Reference the [list of all available commands, aliases and parameters](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Panamax-Installer-Commands) on how to change your VM's specs to increase CPUs and RAM.

## Mac OS X 10.9.0 or higher
_Before installing, please review these [installation notes.](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Installing-Panamax#panamax-client---local-installation-recommended)_

To install Panamax on Mac, use [Homebrew](http://brew.sh/). Here are the steps:

On your terminal window, run:

`$ brew install http://download.panamax.io/installer/brew/panamax.rb`

Install and run Panamax! 

`$ panamax init`

After the installation, Panamax will open a browser window automatically.
For a menu of all commands available to you, simply run:

`$ panamax`

[List of all available commands, aliases and parameters](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Panamax-Installer-Commands) for the Panamax Installer.
[Release Notes](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Release-Notes)

### Upgrade Panamax
To upgrade Panamax to a newer version please run the following on your terminal window:

`brew upgrade http://download.panamax.io/installer/brew/panamax.rb && panamax reinstall`

To check the version of Panamax, run the following on your terminal window:

`panamax info`

##Ubuntu Linux 12.04 or higher
_Before installing, please review these [installation notes.](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Installing-Panamax#panamax-client---local-installation-recommended)_

Ubuntu Desktop Users: Download and run the script as shown.

`$ curl  http://download.panamax.io/installer/ubuntu.sh | bash`

After the installation, you can open a browser window at http://panamax.local:3000/, to view Panamax.

For a menu of all commands available to you, simply run:

`$ panamax`

[List of all available commands, aliases and parameters](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Panamax-Installer-Commands) for the Panamax Installer.
[Release Notes](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Release-Notes)

## Debian 6.0 or higher

See [Ubuntu instructions.](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Installing-Panamax#ubuntu-linux-1204-or-higher)

## Windows with Cgywin
_Before installing, please review these [installation notes.](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Installing-Panamax#panamax-client---local-installation-recommended)_


FIRST: install vagrant >= 1.6 on your windows OS and install virtualbox >= 1.2 on your windows OS.

Then, in your cygwin terminal, symlink your Virtualbox.exe and VBoxManage.exe files like so:
```
ln -s /cygdrive/c/Program\ Files/Oracle/VirtualBox/VirtualBox.exe /usr/bin/virtualbox
ln -s /cygdrive/c/Program\ Files/Oracle/VirtualBox/VBoxManage.exe /usr/bin/VBoxManage
```
Panamax will then use the native windows application.

You may also need to install additional cygwin packages.  I had to add the `bc` package as well.  You can do that by adding `apt-cyg` or by re-running the cygwin installer (this installer won't overwrite anything, just add packages).   To add packages (like the `bc` package) inside the cygwin terminal run:
```
wget http://apt-cyg.googlecode.com/svn/trunk/apt-cyg
chmod +x apt-cyg
mv apt-cyg /usr/local/bin/
apt-cyg install bc
```

Please use the following instructions to install Panamax on Windows that has [Cygwin](https://cygwin.com/install.html) installed. A big thanks to Alan Kent for providing the instructions via his [blog post](http://alankent.wordpress.com/2014/08/13/playing-with-docker-coreos-and-panamax-on-windows/).

Run the following commands from inside a [Cygwin](https://cygwin.com/install.html) bash terminal window:

```
$ mkdir ~/.panamax
$ cd ~/.panamax
$ curl -O http://download.panamax.io/installer/panamax-latest.tar.gz
$ mkdir -p ~/.panamax && tar -C ~/.panamax -zxvf panamax-latest.tar.gz
$ ./panamax init
```

After the installation, you can open a browser window at http://panamax.local:3000/, to view Panamax.

For a menu of all commands available to you, simply run:

`$ ./panamax`

[List of all available commands, aliases and parameters](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Panamax-Installer-Commands) for the Panamax Installer.
[Release Notes](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Release-Notes)


## Cloud Provider Installation - Panamax Client

**NOTE:**  The [remote agent capability](http://www.centurylinklabs.com/panamax-status-update-remote-agent-and-adapters/) provides the ability to deploy an application to remote infrastructure targets.  It is not recommended to install the Panamax Client on Cloud Providers although it is still possible and the previous instructions remain here:  [Installing the Panamax Client(UI/API) on a Cloud Provider](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Cloud-Provider-Installation-of-Panamax-Client).

# Panamax Remote Agent Installation 
_Short URL:_ [http://j.mp/pmx_install_agent](http://j.mp/pmx_install_agent)

Installing the Panamax Remote Agent allows you to deploy your application directly from your Panamax client to a deployment endpoint. The following describes the process for preparing your deployment environment and how to install the Panamax Remote Agent and adapter for your specific orchestrator.

## Setup your deployment environment
Panamax allows you to deploy your application to any node or cluster of nodes utilizing our remote agent. The first step is to setup your deployment endpoint on the cloud provider of your choice with the orchestrator of your choice. The following links should provide some guidance on how to setup an environment with specific orchestrators that Panamax currently supports:

###Kubernetes
[Kubernetes Project Home](https://github.com/GoogleCloudPlatform/kubernetes/) - lots of guides for setting up Kubernetes on different environments

[Getting started on Google Compute Engine](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/gce.md)

[How To Install and Configure Kubernetes on top of a CoreOS Cluster](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-kubernetes-on-top-of-a-coreos-cluster)

[Single-node CoreOS Setup](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/coreos_cloud_config.md)

[Multi-node CoreOS Cluster Setup](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/coreos_cloud_config.md)

[Debugging FAQ](https://github.com/GoogleCloudPlatform/kubernetes/wiki/Debugging-FAQ)

###Fleet/etcd
[Running CoreOS on Google Compute Engine](https://coreos.com/docs/running-coreos/cloud-providers/google-compute-engine/)

[Running CoreOS on EC2](https://coreos.com/docs/running-coreos/cloud-providers/ec2/)

[Building CoreOS Server Cluster on the CenturyLink Cloud](https://t3n.zendesk.com/entries/47064274-Building-CoreOS-Server-Cluster-on-the-CenturyLink-Cloud)

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
4. Select (1) - `init`, to begin the installation process
5. Follow the prompts, including choosing your adapter, adding your API endpoint (typically a private IP) and Panamax Remote Agent endpoint (typically a public IP)
6. Copy the displayed token (needed when adding the deployment endpoint to the client)

_NOTE:_ You can run ``$ ./pmx-agent`` to reinstall, upgrade and run other options for the remote agent. The folder is located under root home. Access the folder, follow the following steps:

1. ``$ sudo su``
2. ``$ cd ~/pmx-agent``
3. ``$ ./pmx-agent``

## Adding a deployment endpoint to the client
With you copied token, browse to the Dashboard on the Panamax client and select Remote Deployment Targets:

![image](http://panamax.ca.tier3.io/panamax_ui_wiki_screens/Remote_target-dashboard.png)

Click on _Add a new Remote Deployment Target_, enter a friendly name and your copied token from the Remote Agent:

![image](http://panamax.ca.tier3.io/panamax_ui_wiki_screens/Add_target.png)

Click _Create Remote Deployment Target_ to save. The deploy target is now available to deploy applications.