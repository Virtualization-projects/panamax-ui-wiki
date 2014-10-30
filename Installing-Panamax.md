_Short URL: [http://j.mp/pmx_install](http://j.mp/pmx_install)_

We recommend the Panamax Client (UI/API) is installed locally but it can theoretically be installed anywhere [CoreOS can run.](http://coreos.com/docs/) The  instructions on this page represent known tested installations. 

[![Installing Panamax Video](http://img.youtube.com/vi/15IKkYCfymk/0.jpg)](http://www.youtube.com/watch?v=15IKkYCfymk) 
***
# Local Installation (Recommended) 
Panamax is currently in BETA

**Panamax Client - Local Client Installation Requirements**

[VirtualBox 4.3](https://www.virtualbox.org/wiki/Downloads/) or higher

[Vagrant 1.6](http://www.vagrantup.com/downloads.html/) or higher

The Panamax installer creates a VM in VirtualBox called _panamax-vm_. This VM is built on [http://coreos.com/docs/running-coreos/platforms/vagrant/](CoreOS.)

**NOTE**: The _panamax-vm_ runs with 2 CPUs and 1GB of RAM by default. These may need to be increased based on the number of containers you have running on your VM or the resources they consume. Templates may specify system recommendations for proper performance in their documentation. Reference the [list of all available commands, aliases and parameters](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Panamax-Installer-Commands) on how to change your VM's specs to increase CPUs and RAM.

## Mac OS X 10.9.0 or higher
_Before installing, please review these [installation notes.](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Installing-Panamax#local-installation)_

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
_Before installing, please review these [installation notes.](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Installing-Panamax#local-installation)_

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
_Before installing, please review these [installation notes.](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Installing-Panamax#local-installation)_


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


# Cloud Provider Installation

**NOTE: Panamax does not provide any authentication measures out of the box. When installing Panamax in any sort of public space both the API and the UI ought to be secured.**

Do not treat any data on a publicly accessible provisioned Panamax server as private.

**When installing Panamax on a cloud provider, ensure the CoreOS version matches the version listed on our [release notes page](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Release-Notes) for our stable release.**

**When installed on a cloud provider, Panamax will not alert you of updates. Check our [release notes page](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Release-Notes) frequently for information about the latest releases and features.**

## CenturyLink Cloud
This is a guide to installing Panamax on [CenturyLink Cloud](http://www.centurylinkcloud.com)

### Create a DHCP_PXE Server in CLC
1. Login to your [CLC control panel](https://control.tier3.com)
1. Add a VLAN (e.g. CoreOSVLAN) in the data center you want to deploy Panamax to. You need to add a VLAN specific for panamax. [Here are specific steps to add a VLAN.](https://t3n.zendesk.com/entries/21806469-Creating-and-Deleting-VLANs)
1. Browse to Blueprints Library and deploy the **DHCP-PXE Server** blueprint. _NOTE_: Be sure to select the VLAN created above for network.

### Create a CoreOS Server with Panamax
1. Deploy the **CoreOS Server with Panamax** blueprint being sure to select the VLAN created above for the network.
1. For the **Execute on Server** option, be sure to select the name of the DHCP_PXE server you created in the previous step, not the CoreOS machine you are currently creating. **This is a very important step.** The server credentials entered will **NOT** actually be used to login to the server as you will use SSH key authorization to access your CoreOS servers.
1. After the Blueprint task is complete, view the **build log** and search for the text **"IP Address of CoreOS Server"** to obtain the IP address that was used for deploying the server. Take note of this address for future use as it will not be displayed in the control panel.

Panamax will be installed on the CoreOS machine by this point. 

### Final Configuration
1. Add public access for Panamax by adding a public IP to the DHCP_PXE server from the control panel
1. Open ports **22**, **3000** for the Public IP
1. SSH to the DHCP_PXE server on the Public IP with root & password given during blueprint creation: `$ ssh root@<public IP>`
1. Make note of the IP of the CoreOS VM by running the following: `$ tail /var/log/syslog`.
1. Make note of the private IP of the DHCP_PXE server by running: `$ ifconfig`.
1. Change the sshd_config file on the DHCP_PXE server so that public access is allowed:
    * Add "GatewayPorts yes" to the file **/etc/ssh/sshd_config**
    * Run: `$ service ssh restart`
1. Run the following command on the DHCP_PXE server to give access to 3000 from public IP: `$ ssh -f -N root@<DHCP server private IP> -R *:3000:<Core OS Vm IP>:3000`
1. Access Panamax at http://_public-IP_:3000 !

_NOTE_: If you prefer not to expose a public IP to access Panamax, [see these instructions.](https://t3n.zendesk.com/entries/20914433-How-To-Configure-Client-VPN)

### Reconfiguration after DHCP_PXE reboots
When ever your DHCP_PXE server is rebooted, you need to restart the SSH tunnels between the DHCP server and the CoreOS VM. Following these steps to re-establish:

1. Make note of the CoreOS private IP address by running the following on the DHCP_PXE server: `$ cat /var/log/syslog` - The IP will be included in the last entry.
1. Run the following command on the DHCP_PXE server adding the IP address from above: `$ ssh -f -N root@<DHCP_server_private_IP> -R *:3000:<CoreOS_private_IP>:3000`
 
## Amazon Web Services - EC2
This is a guide to installing Panamax on [EC2 CoreOS AMI](http://coreos.com/docs/running-coreos/cloud-providers/ec2/)

### Create a CoreOS VM in EC2
1. Choose **Launch Instance** in EC2 Dashboard 
1. Select **Community AMIs** in AMI Selection wizard and search for **CoreOS**. Recommended images are: _CoreOS-stable-444.4.0-hvm _ and _CoreOS-stable-444.4.0_.
1. Make sure the AMI has the following specs at **minimum**: 1 vCPU, 4 GB RAM, 40 GB HDD. For example, **m3.medium** at a minimum.
1. Select Auto-assign Public IP as **enabled**.
1. Change storage to Root (/dev/xvda) to 40GB General Purpose (SSD)
1. Configure the following security rules by opening the following ports:
   * 22,TCP, Source= Anywhere
   * 3000, TCP, Source = Anywhere
   * If your App requires additional ports, make sure to go       back and add them.) 
1. Create and save your key as a local file (e.g coreos-private-key.pem).
1. Run `$ chmod 400 coreos-private-key.pem` to change permission on .pem file to use in SSH

### Install Panamax

1. Once the VM is created, SSH into the box with:

   `$ ssh -i  coreos-private-key.pem core@<AMI public DNS ID or Public IP of the VM>`

1. Run: `$ sudo su`
1. Download & unzip the latest setup script from [http://download.panamax.io/installer/panamax-latest.tar.gz](http://download.panamax.io/installer/panamax-latest.tar.gz):

    `$ curl -O http://download.panamax.io/installer/panamax-latest.tar.gz && mkdir -p /var/panamax && tar -C /var/panamax -zxvf panamax-latest.tar.gz`
1. Change to the /var/panamax directory: `$ cd /var/panamax`
1. Run: `$ ./coreos install --stable`
1. Once the installer completes, you can access panamax at: `http:// _Public IP_ :3000/`

[See our Known Issues page for items related to an EC2 installation.](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Known-Issues#running-on-ec2)

## Brightbox

### Create a CoreOS VM on Brightbox
1. Create a new server group with the following Firewall rules:
   * 22,TCP, Source= Anywhere
   * 3000, TCP, Source = Anywhere
   * Any additional ports your application requires
1. On the Image Library dashboard, search for the CoreOS 444.4.0 image: _img-fpeuh_, and create a new Cloud Server
   * Select the server cloud create above
   * Use, at minimum, the Small (2GB) Server type
1. Create a new public IP on the Cloud IPs dashboard and assign it to the newly created CoreOS server

### Install Panamax

1. Once the VM is created, SSH into the box. _NOTE_: you previously needed to upload your public key to Brightbox.

   `$ ssh core@<Public IP of the VM>`

1. Run: `$ sudo su`
1. Download & unzip the latest setup script from [http://download.panamax.io/installer/panamax-latest.tar.gz](http://download.panamax.io/installer/panamax-latest.tar.gz):

    `$ curl -O http://download.panamax.io/installer/panamax-latest.tar.gz && mkdir -p /var/panamax && tar -C /var/panamax -zxvf panamax-latest.tar.gz`
1. Change to the /var/panamax directory: `$ cd /var/panamax`
1. Run: `$ ./coreos install --stable`
1. Once the installer completes, you can access panamax at: `http:// _Public IP_ :3000/`

## Digital Ocean

### Create a CoreOS VM on Digital Ocean
1. Be sure to upload your Public ssh key to Digital Ocean prior to creating your CoreOS VM
1. Create a new droplet and select CoreOS 444.4.0 - Use, at minimum, the 2GB server type
1. After the droplet is created, view its detail and via the Settings section, select the Networking tab - make note of your public IP address.

### Install Panamax

1. Once the VM is created, SSH into the box. _NOTE_: you previously needed to upload your public key to Digital Ocean.

   `$ ssh core@<Public IP of the VM>`

1. Run: `$ sudo su`
1. Download & unzip the latest setup script from [http://download.panamax.io/installer/panamax-latest.tar.gz](http://download.panamax.io/installer/panamax-latest.tar.gz):

    `$ curl -O http://download.panamax.io/installer/panamax-latest.tar.gz && mkdir -p /var/panamax && tar -C /var/panamax -zxvf panamax-latest.tar.gz`
1. Change to the /var/panamax directory: `$ cd /var/panamax`
1. Run: `$ ./coreos install --stable`
1. Once the installer completes, you can access panamax at: `http:// _Public IP_ :3000/`

## Vultr

### Create a CoreOS VM on Vultr
1. [Follow these instructions](https://coreos.com/docs/running-coreos/cloud-providers/vultr/) using `http://stable.release.core-os.net/amd64-usr/444.4.0/` in the `set base-url` statement.
1. When creating your VM, be sure to select the 2CPU, 2GB flavor at a minimum.
1. Make note of the public IP for your CoreOS VM after its created

### Install Panamax

1. Once the VM is created, SSH into the box.

   `$ ssh core@<Public IP of the VM>`

1. Run: `$ sudo su`
1. Download & unzip the latest setup script from [http://download.panamax.io/installer/panamax-latest.tar.gz](http://download.panamax.io/installer/panamax-latest.tar.gz):

    `$ curl -O http://download.panamax.io/installer/panamax-latest.tar.gz && mkdir -p /var/panamax && tar -C /var/panamax -zxvf panamax-latest.tar.gz`
1. Change to the /var/panamax directory: `$ cd /var/panamax`
1. Run: `$ ./coreos install --stable`
1. Once the installer completes, you can access panamax at: `http:// _Public IP_ :3000/`

## OpenStack
[Running Panamax In CoreOS On Top Of OpenStack](http://cloudssky.com/en/blog/Running-Panamax-In-CoreOS-On-Top-Of-OpenStack/)

## Rackspace

### Create a CoreOS VM on Rackspace

1. Go to the [Cloud Control Panel](https://mycloud.rackspace.com/)
1. Click Create Server
  1. Name: panamax
  1. Image: Linux > CoreOS > Stable
  1. Flavor: Performance 1 > 4 GB Performance
  1. Advanced Options: (Optional) Create or use an existing SSH key for this server
1. Click Create Server
  1. Note the password and IPv4 address (when it appears)

### Install Panamax

1. Once the VM is created, SSH into the box. If you used an SSH key, you can use that here instead of the password.

   `$ ssh root@<Public IPv4 of the VM>`

1. Download & unzip the latest setup script from [http://download.panamax.io/installer/panamax-latest.tar.gz](http://download.panamax.io/installer/panamax-latest.tar.gz):

    `$ curl -O http://download.panamax.io/installer/panamax-latest.tar.gz && mkdir -p /var/panamax && tar -C /var/panamax -zxvf panamax-latest.tar.gz`

1. Change to the /var/panamax directory: `$ cd /var/panamax`
1. Run: `$ ./coreos install --stable`
1. Once the installer completes, you can access panamax at: `http://<Public IPv4 of the VM>:3000/`

## Google Compute Engine

First, install the [Google Cloud SDK][gcutil] and log in.

```console
$ gcloud auth login
```

Next, create a project in the [developer console][devcons]. The project ID 
should look something like `golden-cove-408`. For convenience, you should set 
that as the default in the tools.

```console
$ gcloud config set project golden-cove-408
```

Now enable billing. *Note that anything you do past this point can and will 
cost you money*. Navigate to the project console and choose the billing 
settings, then click enable.

In order to create GCE machines, you need to initialize it in the browser at 
least once. Go to Compute->Compute Engine->VM instances and wait for it to show 
up with a screen that has a "create an instance" button.

The included `cloud-config.yaml` file will set up the CoreOS machine to 
automatically download and install Panamax on the first boot.

Run `./provision.sh` (from: https://github.com/CenturyLinkLabs/panamax-contrib/tree/master/providers/gce) with the first argument as the hostname of the machine. 
The script will automatically provision a 50GB data drive for docker containers 
as well as the machine Panamax will run on.

Before provisioning, you might want to add a user config to the user-data. Here 
is an example:

```yaml
users:
  - name: xena
    gecos: Xena Cadenza
    groups:
      - sudo
      - docker
    coreos-ssh-import-github: Xe
```

### PLEASE NOTE THAT IF YOU DO NOT DO THIS STEP YOU MAY NOT BE ABLE TO SSH INTO YOUR MACHINE. YOU HAVE BEEN WARNED.

```console
$ ./provision.sh caml
```

In testing the install of Panamax takes about 15 minutes. If you want you can 
monitor this install by ssh'ing into the machine as soon as you can and then 
running `journalctl -u install-panamax.service -f`.

As soon as the unit finishes, Panamax will be installed on your GCE node.

[gcutil]: https://developers.google.com/compute/docs/gcutil/#install
[devcons]: https://console.developers.google.com/project