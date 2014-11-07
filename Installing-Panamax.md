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