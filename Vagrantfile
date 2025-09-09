# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.define "kubemaster" do |kubemaster|
    kubemaster.vm.hostname = "kubemaster"
    kubemaster.vm.box = "ubuntu/jammy64"
    kubemaster.vm.network "private_network", ip: "192.168.56.20"
    kubemaster.vm.provision "shell", inline: "sudo apt update && sudo DEBIAN_FRONTEND=noninteractive apt install -y apt-cacher-ng"
    kubemaster.vm.provision "shell", inline: "echo 'Acquire::http { Proxy \"http://192.168.56.20:3142\"; };' | sudo tee /etc/apt/apt.conf.d/99proxy"
    kubemaster.vm.provision "shell", inline: "echo 'Acquire::Retries \"5\";' | sudo tee /etc/apt/apt.conf.d/80-retries"
    kubemaster.vm.provision "shell", path: "provision.sh"
    kubemaster.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
      vb.cpus = 2
    end
  end

  (1..2).each do |i|
    config.vm.define "worker#{i}" do |worker|
      worker.vm.hostname = "worker#{i}"
      worker.vm.box = "ubuntu/jammy64"
      worker.vm.network "private_network", ip: "192.168.56.2#{i}"
      worker.vm.provision "shell", inline: "echo 'Acquire::http { Proxy \"http://192.168.56.20:3142\"; };' | sudo tee /etc/apt/apt.conf.d/99proxy"
      worker.vm.provision "shell", path: "provision.sh"
      worker.vm.provider "virtualbox" do |vb|
        vb.memory = "2048"
        vb.cpus = 2
      end
    end
  end
end

