Panamax can be installed locally or theoretically anywhere [CoreOS can run.](http://coreos.com/docs/) The  instructions on this page represent known tested installations. 

[![Installing Panamax Video](http://img.youtube.com/vi/15IKkYCfymk/0.jpg)](http://www.youtube.com/watch?v=15IKkYCfymk) 
***
# Local Installation
Panamax is currently in BETA

**Local Installation Requirements**

[VirtualBox 4.2](https://www.virtualbox.org/wiki/Downloads/) or higher

[Vagrant 1.6](http://www.vagrantup.com/downloads.html/) or higher

The Panamax installer creates a VM in VirtualBox called _panamax-vm_. This VM is built on [http://coreos.com/docs/running-coreos/platforms/vagrant/](CoreOS.)

**NOTE**: The _panamax-vm_ runs with 2 CPUs and 1GB of RAM by default. These may need to be increased based on the number of containers you have running on your VM or the resources they consume. Templates may specify system recommendations for proper performance in their documentation. Reference the [list of all available commands, aliases and parameters](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Panamax-Installer-Commands) on how to change your VM's specs to increase CPUs and RAM.

## Mac OS X 10.9.0 or higher

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

##Ubuntu Linux 12.04 or higher

Ubuntu Desktop Users: Download and run script as shown.

`$ \curl  http://download.panamax.io/installer/ubuntu.sh | bash`

After the installation, Panamax will open a browser window automatically.
For a menu of all commands available to you, simply run

`$ panamax`

[List of all available commands, aliases and parameters](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Panamax-Installer-Commands) for the Panamax Installer.
[Release Notes](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Release-Notes)

# Cloud Provider Installation

*NOTE: Panamax does not provide any authentication measures out of the box. When installing Panamax in any sort of public space both the API and the UI ought to be secured.*

## Amazon Web Services - EC2
This is a guide to installing Panamax on [EC2 CoreOS AMI](http://coreos.com/docs/running-coreos/cloud-providers/ec2/)

### Create a CoreOS VM in EC2
1. Choose **Launch Instance** in EC2 Dashboard 
1. Select **AWS Marketplace** in AMI Selection wizard and search for **CoreOS**.
1. Make sure the AMI has the following specs at **minimum**: 1 vCPU, 4 GB RAM, 20 GB HDD
1. Configure the following security rules:
    
    Open Ports 22, 3000 from anywhere. (These are for Panamax. If your App requires additional ports, make sure to go       back and add them.) 
1. Add a public IP to the VM
1. Save _private-key.pem_ to a local file (e.g coreos-private-key.pem).

### Install Panamax
Once the VM is created, SSH into the box with:

`$ ssh -i  coreos-private-key.pem core@<AMI public DNS ID or Public IP of the VM>`

1. Download & unzip the latest setup script from [http://download.panamax.io/installer/pmx-installer-latest.zip](http://download.panamax.io/installer/pmx-installer-latest.zip)

1. Run:
`$ ./coreos install --stable`

1. Once the installer completes, you can access panamax at http:// _Public IP_ :3000/