# -*- mode: ruby -*-
# vi: set ft=ruby :

#
#Ubuntu Xenial mit Docker
#

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/xenial64"
  
  # Docker Provisioner
  config.vm.provision "docker" do |d|
  end  

  # Create a public network, which generally matched to bridged network.
  config.vm.define "mysql" do |mysql|
	db.vm.provider "virtualbox" do |vb|
	  vb.memory = "1024"  
	end
    db.vm.hostname = "mysql"
    db.vm.network "private_network", ip: "192.168.100.100"
	db.vm.network "forwarded_port", guest:3376 , host:3376, auto_correct: false
	db.vm.network "forwarded_port", guest:443 , host:443, auto_correct: false
  end
  
  config.vm.define "wordpress" do |web|
    web.vm.hostname = "wordpress"
    web.vm.network "private_network", ip:"192.168.100.101"
	web.vm.network "forwarded_port", guest:80, host:8080, auto_correct: true
	web.vm.provider "virtualbox" do |vb|
	  vb.memory = "1024"  
	end     
  end 
   
end