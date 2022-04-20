# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "centos/7"
  config.vm.box_version = "2004.01"
  
  config.vm.provider "virtualbox" do |v|
  v.memory = 256
  v.cpus=1
end
  config.vm.define "nfss" do |nfss|
  nfss.vm.network "private_network", ip: "192.168.56.105",
  virtualbox__intnet: "net1"
  nfss.vm.hostname = "CentOS_ans-server"
end

  config.vm.define "nfsc" do |nfsc|
  nfsc.vm.network "private_network", ip: "192.168.56.104",
  virtualbox__intnet: "net1"
  nfsc.vm.hostname = "CentOS_ans-client-01"
  end
end
