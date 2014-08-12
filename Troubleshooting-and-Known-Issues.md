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
$ panamax delete
$ panamax init 
```

##### Panamax installer and Vagrant boot issues 
In certain cases, if the version of VirtualBox is not up-to-date or misconfigured, Panamax installer gives issues. 

```
$ panamax init

Message -> ==> panamax-vm: Running 'pre-boot' VM customizations...
==> panamax-vm: Booting VM...
==> panamax-vm: Waiting for machine to boot. This may take a few minutes...
The guest machine entered an invalid state while waiting for it to boot. Valid states are 'starting, running'. The machine is in the 'poweroff' state. Please verify everything is configured properly and try again.  
```

Please reinstall VirtualBox to resolve the issue. 

##### Message: "A VirtualBox machine with the name 'panamax-vm' already exists."
This can happen if your CoreOS VM stops responding. You can delete the _panamax-vm_ by running `panamax delete`, to uninstall Panamax and then performing a `panamax init` to reinstall Panamax and the VM.

##### Panamax installer fails due to port conflicts on 8888

Panamax installer by default tries to use port 8888 and 8889 to host the Panamax application. So, if that port is being used by some other VM on your machine, you will get a message as shown below:

```
Vagrant cannot forward the specified ports on this VM, since they
would collide with some other application that is already listening
on these ports. The forwarded port to 8888 is already in use
on the host machine.

To fix this, modify your current projects Vagrantfile to use another
port. Example, where '1234' would be replaced by a unique host port:

  config.vm.network :forwarded_port, guest: 3000, host: 1234

Sometimes, Vagrant will attempt to auto-correct this for you. In this
case, Vagrant was unable to. This is usually because the guest machine
is in a state which doesn't allow modifying port forwarding.
VM Creation failed. Exiting.
```

To resolve the issue, you can change your existing port mappings or install Panamax using custom ports by:

``` panamax init -ppUi=7777 -ppApi=7778 ```

where, 7777 and 7778 are the new ports for the UI and the API respectively.

See `panamax help` for other install options.

# Launching Applications/Services

## Known Issues

##### Any service whose name contains the pattern _{a-f}{a-f} will fail to start and crash the systemd service

This is a [known issue](https://github.com/coreos/go-systemd/pull/49) with the CoreOS implementation of systemd. The issue has been resolved and the fix will be available in the next release of CoreOS. 