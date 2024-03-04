# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

#Notes
#user name for VMs is created as vagrant

#<vagrant box add bento/ubuntu-20.04> to add OS image to internal repository
#<vagrant up> to start VMs (run at the same folder where this file is, or pass file as argument)
#<vagrant halt> to stop VMs
#<vagrant destroy> to remove VMs

#You should only need change the first two/three variables in this file

#Variables
Number_VMs = 1 #number VMs to launch
IP_RANGE= "192.168.56" #Change me!!!!
PUBLIC_KEY_PATH = "/opt/id_rsa.pub" #Change me!!
READ_PUBLIC_KEY = File.read(File.expand_path(PUBLIC_KEY_PATH)).strip

Vagrant.configure("2") do |config|

  config.vm.box = "bento/ubuntu-18.04"

# Provider-specific configuration
  config.vm.provider "virtualbox" do |vb|
    # Customize the amount of memory on the VM:
    vb.memory = "1024"
    vb.cpus = 1
  end

  (1..1).each do |i|
    config.vm.define "server#{i}" do |node|
      node.vm.hostname = "vm#{i}"
      node.vm.network :private_network, ip: "#{IP_RANGE}.#{100+i}"
    end
  end

 config.ssh.insert_key = false

 config.vm.provision "shell", inline: <<-SHELL
        echo "#{READ_PUBLIC_KEY}" >> /home/vagrant/.ssh/authorized_keys
        sed -i 's/#PubkeyAuthentication yes/PubkeyAuthentication yes/g' /etc/ssh/sshd_config
        sed -i 's/#PasswordAuthentication yes/PasswordAuthentication no/g' /etc/ssh/sshd_config
        systemctl restart sshd
        apt update -y 
        apt upgrade -y
        apt install vim curl -y
      SHELL
end
