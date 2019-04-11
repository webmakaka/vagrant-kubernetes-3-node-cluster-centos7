# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_NO_PARALLEL'] = 'yes'

Vagrant.configure(2) do |config|

  config.vm.box = "centos/7"
  config.hostmanager.enabled = true
  config.hostmanager.include_offline = true

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = 2
  end

  config.vm.provision "shell", path: "bootstrap.sh"

  # Kubernetes Master Server
  config.vm.define "master.k8s" do |c|
    c.vm.hostname = "master.k8s"
    c.vm.network "private_network", ip: "192.168.0.10"
    c.vm.provision "shell", path: "bootstrap_master.sh"
  end

  NodeCount = 2

  # Kubernetes Worker Nodes
  (1..NodeCount).each do |i|
    config.vm.define "node#{i}.k8s" do |c|
      c.vm.hostname = "node#{i}.k8s"
      c.vm.network "private_network", ip: "192.168.0.1#{i}"
      c.vm.provision "shell", path: "bootstrap_node.sh"
    end
  end
end
