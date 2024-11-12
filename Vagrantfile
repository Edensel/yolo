# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "centos/7"
 
  config.vm.network "public_network"

  # Sync the current directory with the VM
  config.vm.synced_folder ".", "/vagrant"


    # Add a shell provisioner to install Docker and Docker Compose
  config.vm.provision "shell", inline: <<-SHELL
    # Update package list and install Docker
    sudo apt-get update
    sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    sudo apt-get update
    sudo apt install net-tools -y
    sudo apt-get update
    sudo apt-get install -y chrony
    sudo apt-get update
    sudo apt-get install -y docker-ce docker-ce-cli containerd.io

    # Add vagrant user to the docker group
    sudo usermod -aG docker vagrant

    # Install pip for Python 3 and Docker Compose
    sudo apt-get install -y python3-pip
    pip3 install docker-compose
  SHELL
  
  # Use Ansible local provisioning
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "provisioning/playbook.yml"
  end

  config.vm.network "forwarded_port", guest: 3000, host: 8080
  config.vm.network "forwarded_port", guest: 5000, host: 5001
  
end