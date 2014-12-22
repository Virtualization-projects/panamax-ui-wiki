_Short URL: [http://j.mp/pmx_install](http://j.mp/pmx_install)_

Panamax is comprised of the Panamax Client which can be used to create new Panamax Templates and deploy Templates and Images locally.  If you require to deploy templates to remote infrastructure (not local) be sure to also [install the Panamax Remote Agent](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Panamax-Remote-Agent-Installation) on the remote infrastructure you need to deploy to. 

[![Installing Panamax Video](http://img.youtube.com/vi/15IKkYCfymk/0.jpg)](http://www.youtube.com/watch?v=15IKkYCfymk) 
***
# Panamax Client - Local Installation (Recommended) 
Panamax is currently in BETA

**Local Client Installation Requirements**

[VirtualBox 4.3](https://www.virtualbox.org/wiki/Downloads/) or higher

[Vagrant 1.6](http://www.vagrantup.com/downloads.html/) or higher

The Panamax installer creates a VM in VirtualBox called _panamax-vm_. This VM is built on [CoreOS](http://coreos.com/docs/running-coreos/platforms/vagrant/).

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

## Arch Linux

See [Ubuntu instructions.](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Installing-Panamax#ubuntu-linux-1204-or-higher)

## Debian 6.0 or higher

See [Ubuntu instructions.](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Installing-Panamax#ubuntu-linux-1204-or-higher)

## Windows with Cgywin
_Before installing, please review these [installation notes.](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Installing-Panamax#panamax-client---local-installation-recommended)_

### Method #1
#### Prerequisites
* Windows 7.1+ 64bit
* [Cygwin](https://cygwin.com/setup-x86_64.exe) (with/add rsync and openssh packages)
* [Vagrant 1.6.5](https://dl.bintray.com/mitchellh/vagrant/vagrant_1.6.5.msi) _NOTE: V1.7.0 does not work_ (tested on 1.6.5) 
* [VirtualBox 4.3.10+](http://download.virtualbox.org/virtualbox/4.3.20/VirtualBox-4.3.20-96997-Win.exe) (tested on 4.3.20)

#### Install CoreOS VM in VirtualBox
1. Open Cygwin64 terminal on Windows machine
2. Run `$ mkdir panamax-vagrant && cd panamax-vagrant` 
3. Run `$ vagrant -v`, `$ rsync --version` to confirm that they are working and are in your PATH
4. Run `$ curl -O http://download.panamax.io/installer/panamax-demo.tar.gz && tar -C . -zxvf panamax-demo.tar.gz && mv Vagrantfile-win Vagrantfile`
5. Run `$ vagrant up` 
6. After install, port forward guest (CoreOS) port 3000 to host (Windows) port 3000 in the VirtualBox GUI | Network | Port Forwarding on the panamax-vm or run on the terminal:

    ```$ VBoxManage controlvm panamax-vm natpf1 rule1,tcp,,3000,,3000```
7. Browse to `localhost:3000` on your windows machine to view Panamax!

#### Reinstall, Restart, Update Panamax
1. Run `$ vagrant ssh` from the `panamax-vagrant ` directory on Windows machine.
2. When on the CoreOS, run `$ sudo su`
3. Change the directory to panamax: `$ cd /var/panamax`
4. Run `$ ./coreos`
5. Make your selection

[List of all available commands, aliases and parameters](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Panamax-Installer-Commands) for the Panamax Installer.
[Release Notes](https://github.com/CenturyLinkLabs/panamax-ui/wiki/Release-Notes)

### Method #2 
FIRST: install vagrant >= 1.6 on your windows OS and install virtualbox >= 1.4.3+ on your windows OS.

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