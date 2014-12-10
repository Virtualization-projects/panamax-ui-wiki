# Installation for Windows
### Prerequisites
* Windows 7.1+ 64bit
* [Cygwin](https://cygwin.com/setup-x86_64.exe) (with/add rsync and openssh packages)
* [Vagrant 1.6.5](https://dl.bintray.com/mitchellh/vagrant/vagrant_1.6.5.msi) _NOTE: V1.7.0 does not work_ (tested on 1.6.5) 
* [VirtualBox 4.3.10+](http://download.virtualbox.org/virtualbox/4.3.20/VirtualBox-4.3.20-96997-Win.exe) (tested on 4.3.20)

## Install CoreOS VM in VirtualBox
1. Open Cygwin64 terminal on Windows machine
2. Run `$ mkdir panamax-vagrant && cd panamax-vagrant` 
3. Run `$ vagrant -v`, `$ rsync --version` to confirm that they are working and are in your PATH
4. Run `$ curl -O http://download.panamax.io/installer/panamax-demo.tar.gz && tar -C . -zxvf panamax-demo.tar.gz && mv Vagrantfile-win Vagrantfile`
5. Run `$ vagrant up` 
6. After install, port forward guest (CoreOS) port 3000 to host (Windows) port 3000 in the VirtualBox GUI | Network | Port Forwarding on the panamax-vm or run on the terminal:

    ```$ VBoxManage controlvm panamax-vm natpf1 rule1,tcp,,3000,,3000```
7. Browse to `localhost:3000` on your windows machine to view Panamax!

## Reinstall, Restart, Update Panamax
1. Run `$ vagrant ssh` from the `panamax-vagrant ` directory on Windows machine.
2. When on the CoreOS, run `$ sudo su`
3. Change the directory to panamax: `$ cd /var/panamax`
4. Run `$ ./coreos`
5. Make your selection