# Installation and Setup
As and when, issues are discovered, this section will be updated.

## Known Issues


##### Panamax installer requires write permissions on the /Users/<username> folder
Panamax needs write access to the /Users/<username> folder otherwise the Panamax installer will fail. Panamax and Vagrant store its configuration in this folder at ~/.panamax and ~/.panamax/.vagrant, so it should be writable.

##### Panamax installer does not specifically force the Vagrant provider for VirtualBox.
Panamax has been tested with Vagrant VirtualBox provider. If you have VMWare provider installed on your machine, you may see some issues while installing. Since we did not force the Vagrant VirtualBox provider in the Panamax installer, Vagrant tries to use the VMWare provider but it cannot find a VMWare formatted vagrant box. Hence the issue.

While we work on to provide a fix, you can set the environment variable VAGRANT_DEFAULT_PROVIDER to 'virtualbox', to resolve the issue. 

##### Panamax reinstall fails if Vagrant CoreOS base box is re-downloaded.
If you are trying to reinstall Panamax, and you choose to re-download the Vagrant CoreOS base box, you may see issues.

While we work on to provide a fix, you can do the following, to resolve the issue:

    panamax delete
    panamax init

