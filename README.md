# Salt Vagrant Demo

A Salt Demo using Vagrant.

## Requirements

This demo requires `libvirt` with at least one supported hypervisor, this guide will be KVM/QEMU specific.
In addition, the [vagrant-libvirt](https://github.com/vagrant-libvirt/vagrant-libvirt) plugin is needed to utilize libvirt:
```sh
vagrant plugin install vagrant-libvirt
```
## Instructions

Run the following commands in a terminal. Vagrant, Git and any guide-specific packages specified in the **Requirements** section  must already be installed.
```sh
git clone https://github.com/stiftcast/salt-vagrant-demo.git
cd salt-vagrant-demo
vagrant up
```

This will download a generic Ubuntu 18.04 image and create three virtual
machines for you. One will be a Salt Master named `master` and two will be Salt
Minions named `minion1` and `minion2`.  The Salt Minions will point to the Salt
Master and the Minion's keys will already be accepted. Because the keys are
pre-generated and reside in the repo, please be sure to regenerate new keys if
you use this for production purposes.

You can then run the following commands to log into the Salt Master and begin
using Salt.
```sh
vagrant ssh master
sudo salt \* test.ping
```
