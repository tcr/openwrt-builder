# OpenWrt Builder

Setup an [OpenWrt](http://www.openwrt.org/] [Buildroot](http://wiki.openwrt.org/doc/howto/buildroot.exigence) buildroot environment inside VirtualBox, build OpenWrt, boot to OpenWrt in VirtualBox.
<img src="http://i.imgur.com/HL4qt.png">

## Install

* Install [VirtualBox](https://www.virtualbox.org)
* Install [Vagrant](http://vagrantup.com/)
* (Optionally) install [vagrant-vbguest](https://github.com/dotless-de/vagrant-vbguest)
* ~~Install [Veewee](https://github.com/jedi4ever/veewee)~~
* Check out the repository.
* Run `vagrant up`
* You may need to run `vagrant reload` to ensure VirtualBox guest additions are setup.
* The Chef provisioner will setup all the build prerequisites and build a VM image. This is slow!
* Once the Chef provisioner completes, you'll have an x86 VirtualBox image in `bin/x86`
* `vagrant ssh` to login
* Code is in `/mnt/openwrt` on the guest. We're not mounting from host to guest because hard links on VirtualBox filesystems don't work on all platforms.
* Run `rake` to setup a VirtualBox image for the OpenWrt build.
* Launch the VM from VirtualBox manager or run `rake vbox_launch`.
* *[from wiki](http://wiki.openwrt.org/doc/howto/virtualbox#set.up.networking.with.clients)* Boot up OpenWrt and add to /etc/config/network
<pre>
config 'interface' 'wan'
        option 'proto' 'dhcp'
        option 'ifname' 'eth1'
</pre>
* Start the wan with `ifup wan`
* DNS resolution should work: `nslookup cnn.com 127.0.0.1`

## Todo

* Get networking to be sane.
* Combine Vagrantfile into Rakefile
* Use Veewee to output a Vagrant image of OpenWrt itself.
* Provisioning for OpenWrt packages.

## Links

* http://wiki.openwrt.org/doc/howto/virtualbox
* `Kernel driver not installed (rc=-1908)` on OSX https://github.com/mitchellh/vagrant/issues/1671
