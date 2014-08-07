Panamax can be installed locally or theoretically anywhere [CoreOS can run.](http://coreos.com/docs/) The  instructions on this page represent known tested installations. 


***


# Local Installation

**Local Installation Requirements**

[VirtualBox 4.2](https://www.virtualbox.org/wiki/Downloads/) or higher

[Vagrant 1.6](http://www.vagrantup.com/downloads.html/) or higher

The Panamax installer creates a VM in VirtualBox called panamax-vm. This VM is built on [http://coreos.com/docs/running-coreos/platforms/vagrant/](CoreOS.)

## Mac OS X:10.9.x and higher

To install Panamax on Mac, use [Homebrew](http://brew.sh/). Here are the steps:
On your terminal window, run:

`$ brew install http://download.panamax.io/installer/brew/panamax.rb`

Install and run Panamax! 

`$ panamax init`

After the installation, Panamax will open a browser window automatically.
For a menu of all commands available to you, simply run

`$ panamax`

##Linux

Ubuntu Desktop Users: Download and run script as shown.

`$ \curl  http://download.panamax.io/installer/ubuntu.sh | bash`

After the installation, Panamax will open a browser window automatically.
For a menu of all commands available to you, simply run

`$ panamax`

[List of all available commands, aliases and parameters](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Panamax-Installer-Commands) for the Panamax Installer.
[Release Notes](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Release-Notes)

# Cloud Provider Installation

*NOTE: panamax does not provide any authentication measures out of the box. When installing panamax in any sort of public space both the API and the UI ought to be secured.*

## CenturyLink Cloud

## Amazon Web Services - EC2
This is a guide to installing Panamax on [EC2 CoreOS AMI](http://coreos.com/docs/running-coreos/cloud-providers/ec2/)

### Create a CoreOS VM in EC2
1. Choose Launch Instance in EC2 Dashboard 
1. Select "AWS Marketplace" in AMI Selection wizard and search for "CoreOS".
1. Make sure the AMI has the following specs at **minimum**: 1 vCPU, 4 GB RAM, 20 GB HDD
1. Configure the following security rules
1. Open Ports 22, 3000 from anywhere. (These are for Panamax. If your App requires additional ports, make sure to go back and add them.) 
1. Add a public IP to the VM
1. Save private-key.pem to a local file (e.g coreos-private-key.pem).

### Install Panamax
1. Once the VM is created, SSH into the box with

`$ ssh -i  coreos-private-key.pem core@<AMI public DNS ID or Public IP of the VM>`

1. Download & unzip the latest setup script from [http://download.panamax.io/installer/pmx-installer-latest.zip](http://download.panamax.io/installer/pmx-installer-latest.zip)

1. Run:
`$ ./coreos install --stable`

1. Once the installer completes, you can access panamax at http:// _Public IP_ :3000/