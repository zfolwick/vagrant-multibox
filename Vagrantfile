# -*- mode: ruby -*-
# vi: set ft=ruby :

#######################################
## this is a minimalist vagrant file ##
#######################################

BOX_IMAGE = "centos/8"
NODE_COUNT = 2

Vagrant.configure("2") do |config|
  config.vm.box = BOX_IMAGE
  config.vm.synced_folder ".", "/vagrant", disabled:true

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
     ###########################################################################
     # this is the magic that gets ssh working.  Normal ssh, not vagrant ssh  ##
     ###########################################################################
     sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
     systemctl restart sshd.service
     ###########################################################
     # This is the stuff that gets ansible to be able to ping ##
     ###########################################################
     sudo yum install wget gcc make -y -q
     wget http://sourceforge.net/projects/sshpass/files/latest/download -O sshpass.tar.gz
     tar -xvf sshpass.tar.gz
     cd sshpass-1.06
     ./configure
     sudo make install

  SHELL

  config.vm.define "master" do |subconfig|
    subconfig.vm.box = BOX_IMAGE
    subconfig.vm.hostname = "master"
    subconfig.vm.network :private_network, ip: "10.0.0.10"
  end

  (1..NODE_COUNT).each do |i|
    config.vm.define "node#{i}" do |subconfig|
      subconfig.vm.box = BOX_IMAGE
      subconfig.vm.hostname = "node#{i}"
      subconfig.vm.network :private_network, ip: "10.0.0.#{i + 10}"
    end
  end
end
