
Common issues will be updated below as they are discovered. If you don't find an answer you might try searching or posting on the [panamax_io Google group](https://groups.google.com/forum/#!forum/panamax_io). 

# Installation and Setup

## Known Issues

##### Panamax installer requires write permissions on the home folder 
Panamax needs write access to the `/Users/username` folder otherwise the Panamax installer will fail. Panamax and Vagrant store its configuration in this folder at `~/.panamax` and `~/.panamax/.vagrant`, so it should be writable.

##### Panamax installer does not specifically force the Vagrant provider for VirtualBox 
Panamax has been tested with Vagrant VirtualBox provider. If you have VMWare provider installed on your machine, you may see some issues while installing. Since we did not force the Vagrant VirtualBox provider in the Panamax installer, Vagrant tries to use the VMWare provider but it cannot find a VMWare formatted vagrant box. Hence the issue.

While we work on to provide a fix, you can set the environment variable `VAGRANT_DEFAULT_PROVIDER` to `virtualbox`, to resolve the issue. 

##### Panamax reinstall fails if Vagrant CoreOS base box is re-downloaded 
If you are trying to reinstall Panamax, and you choose to re-download the Vagrant CoreOS base box, you may see issues.

While we work on to provide a fix, you can do the following, to resolve the issue: 

```
panamax delete
panamax init 
```

##### Panamax installer and Vagrant boot issues 
In certain cases, if the version of VirtualBox is not up-to-date or misconfigured, Panamax installer gives issues. 

```
panamax init

Message -> ==> panamax-vm: Running 'pre-boot' VM customizations...
==> panamax-vm: Booting VM...
==> panamax-vm: Waiting for machine to boot. This may take a few minutes...
The guest machine entered an invalid state while waiting for it to boot. Valid states are 'starting, running'. The machine is in the 'poweroff' state. Please verify everything is configured properly and try again.  
```

Please reinstall VirtualBox to resolve the issue. 

# Launching Applications/Services

## Known Issues

##### Any service whose name contains the pattern _{A-F}{A-F} will fail to start and crash the systemd service

This is a [known issue](https://github.com/coreos/go-systemd/pull/49) with the CoreOS implementation of systemd. The issue has been resolved and the fix will be available in the next release of CoreOS. 