# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANT_API_VERSION = 2

NODES = {
  "build-server"      => ["192.168.22.5", "2215", 2048, 1]
}

Vagrant.configure(VAGRANT_API_VERSION) do |config|
  config.vm.box = "ubuntu/focal64"

  NODES.each do |(name, cfg)|
    ip, ssh_conf, ram, cpu = cfg
    config.vm.define name do |machine|
      machine.vm.network :private_network, ip: ip
      machine.vm.provider "virtualbox" do |v|
        v.name = name
        v.gui = false
        v.memory = ram
        v.cpus = cpu
      end
      machine.vm.provision "shell", path: "update.sh"
      if (name == "build-server")
        machine.vm.provision "shell", path: "build.sh"
      end
      machine.vm.network "public_network"
      machine.vm.network "forwarded_port", host: ssh_conf, guest: 22, id: 'ssh'
    end
  end
end
