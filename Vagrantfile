# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--hwvirtex", "off"]
    vb.customize ["modifyvm", :id, "--nestedpaging", "off"]
    vb.customize ["modifyvm", :id, "--vtxvpid", "off"]
    vb.customize ["modifyvm", :id, "--vtxux", "off"]
  end

  config.vm.define "kubemaster" do |kubemaster|
    kubemaster.vm.hostname = "kubemaster"
    kubemaster.vm.box = "ubuntu/focal64"
    kubemaster.vm.network "private_network", ip: "192.168.99.20"
    kubemaster.vm.provision "shell", path: "provision.sh"
    kubemaster.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
      vb.cpus = 1
    end
  end

  (1..2).each do |i|
    config.vm.define "worker#{i}" do |worker|
      worker.vm.hostname = "worker#{i}"
      worker.vm.box = "ubuntu/focal64"
      worker.vm.network "private_network", ip: "192.168.99.2#{i}"
      worker.vm.provision "shell", path: "provision.sh"
      worker.vm.provider "virtualbox" do |vb|
        vb.memory = "2048"
        vb.cpus = 1
      end
    end
  end

end
