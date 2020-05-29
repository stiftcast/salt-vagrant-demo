# -*- mode: ruby -*-
# vi: set ft=ruby :

BOX_IMAGE = "generic/ubuntu1804"
NETWORK = "192.168.50"
MINIONS = 2

Vagrant.configure("2") do |config|

  # salt master
  config.vm.define :master, primary: true do |master_config|

    master_config.vm.provider "libvirt" do |libvirt|
      libvirt.memory = "2048"
    end

    master_config.vm.box = BOX_IMAGE
    master_config.vm.hostname = "saltmaster"
    master_config.vm.network "private_network", ip: "#{NETWORK}.10"
    master_config.vm.synced_folder "saltstack/salt/", "/srv/salt"
    master_config.vm.synced_folder "saltstack/pillar/", "/srv/pillar"

    master_config.vm.provision :salt do |salt|
      salt.master_config = "saltstack/etc/master"
      salt.master_key = "saltstack/keys/master_minion.pem"
      salt.master_pub = "saltstack/keys/master_minion.pub"
      salt.minion_key = "saltstack/keys/master_minion.pem"
      salt.minion_pub = "saltstack/keys/master_minion.pub"
      salt.seed_master = {
                          "minion1" => "saltstack/keys/minion1.pub",
                          "minion2" => "saltstack/keys/minion2.pub"
                         }

      salt.install_type = "stable"
      salt.install_master = true
      salt.no_minion = true
      salt.verbose = true
      salt.colorize = true
      salt.bootstrap_options = "-P -c /tmp"
    end
  end

  # salt minions...
  (1..MINIONS).each do |i| 
  config.vm.define "minion#{i}" do |node|
    node.vm.box = BOX_IMAGE
    node.vm.hostname = "minion#{i}"
    node.vm.network "private_network", ip: "#{NETWORK}.#{10+i}"

    node.vm.provision :salt do |salt|
      salt.minion_config = "saltstack/etc/minion#{i}"
      salt.minion_key = "saltstack/keys/minion#{i}.pem"
      salt.minion_pub = "saltstack/keys/minion#{i}.pub"
      salt.install_type = "stable"
      salt.verbose = true
      salt.colorize = true
      salt.bootstrap_options = "-P -c /tmp"
    end
  end
end

  # default/global provider options...
  config.vm.provider :libvirt do |libvirt|
    libvirt.driver = "kvm"
    libvirt.connect_via_ssh = false
    libvirt.username = "root"
    libvirt.storage_pool_name = "default"
    libvirt.memory = 1024
    libvirt.cpus = 2
    libvirt.cputopology :sockets => '1', :cores => '2', :threads => '1'
  end
end
