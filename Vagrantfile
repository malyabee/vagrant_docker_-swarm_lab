# -*- mode: ruby -*-
# vi: set ft=ruby :


BOX_BASE = "centos/7"  

$commonscript = <<-SCRIPT
sudo yum update -y
sudo yum install python2 epel-release -y
sudo echo "192.168.22.10	dockermanager.example.com dockermanager" >> /etc/hosts
sudo echo "192.168.22.11   node01.example.com 	node01" >> /etc/hosts
sudo echo "192.168.22.12   node02.example.com      node02" >> /etc/hosts
sudo echo "LANG=en_US.utf-8" > /etc/environment
sudo echo "LC_ALL=en_US.utf-8" >> /etc/environment
SCRIPT

$dockerscript = <<-SCRIPT
sudo yum install wget net-tools -y
sudo wget -O /etc/yum.repos.d/docker-ce.repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install docker-ce -y 
sudo service docker start
sudo chkconfig docker on 
sudo usermod -aG docker vagrant
SCRIPT

Vagrant.configure("2") do |config|
    # Box
	config.vm.define "dockermanager" do |dockermanager|

         # Box
         dockermanager.vm.box = BOX_BASE

         # Memory and CPU allocation
         dockermanager.vm.provider "virtualbox" do |v|
            v.memory = 2048
            v.cpus = 1
         end

         # IP allocation
         dockermanager.vm.network "private_network", ip: "192.168.22.10", virtualbox__intnet: "mynetwork01"

         # Host name allocation
         dockermanager.vm.hostname = "dockermanager.example.com"

         # Installing required packages for ansible controller node
         dockermanager.vm.provision "shell", inline: $commonscript
		 dockermanager.vm.provision "shell", inline: $dockerscript
    end
	
	(1..2).each do |i|
         config.vm.define "node0#{i}" do |node|

          # Box
          node.vm.box = BOX_BASE

          # Memory and CPU allocation
          node.vm.provider "virtualbox" do |v|
              v.memory = 2048
              v.cpus = 2
          end

          node.vm.network "private_network", ip: "192.168.22.1#{i}", virtualbox__intnet: "mynetwork01"

          node.vm.hostname = "node0#{i}.example.com"

          # Installing required packages for  node01
          node.vm.provision "shell", inline: $commonscript
          node.vm.provision "shell", inline: $dockerscript
          
         end
      end
end
