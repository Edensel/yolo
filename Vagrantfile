
Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  
  # Port forwarding
  config.vm.network "forwarded_port", guest: 3000, host: 3000
  config.vm.network "forwarded_port", guest: 5000, host: 5000
  
  # Private network for Ansible
  config.vm.network "private_network", ip: "192.168.33.10"
  
  # VirtualBox specific settings
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = 2
  end

  # Shell provisioning for basic setup
  config.vm.provision "shell", inline: <<-SHELL
    # Install EPEL repository
    sudo yum install -y epel-release
    
    # Install Python3 and pip
    sudo yum install -y python3 python3-pip
    
    # Install Docker
    sudo yum install -y yum-utils
    sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    sudo yum install -y docker-ce docker-ce-cli containerd.io
    
    # Start Docker service
    sudo systemctl start docker
    sudo systemctl enable docker
    
    # Install Docker Compose
    sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    sudo chmod +x /usr/local/bin/docker-compose
    
    # Add vagrant user to docker group
    sudo usermod -aG docker vagrant
  SHELL
end