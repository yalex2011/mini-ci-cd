# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_DEFAULT_PROVIDER'] = 'libvirt'

Vagrant.configure("2") do |config|
  # config.vm.box = "generic/ubuntu1904"
  config.vm.box = "metal/ubuntu-18.10-docker"
  config.ssh.forward_agent = true
  config.vm.define "com"
  config.vm.hostname = "example.com"
  # config.vm.network "private_network", ip: "192.168.121.160"

  config.vm.provider :libvirt do |libvirt|
    libvirt.default_prefix = "example"
    libvirt.memory = 2048
    libvirt.cpus = 2
    # libvirt.storage :file, :size => '20G'
  end

  config.vm.provision "shell", inline: <<-SHELL
    # sudo apt update
    # sudo apt upgrade -y
    sudo apt install -y git curl
    # remove docker
    sudo apt remove docker docker-engine docker.io containerd runc
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    sudo apt update
    sudo apt -y install docker-ce docker-ce-cli containerd.io
    sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    sudo chmod +x /usr/local/bin/docker-compose
    sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
    sudo usermod -aG docker $USER
    newgrp docker
    hostnamectl set-hostname example.com
    git clone https://github.com/yalex2011/mini-ci-cd.git
    cd mini-ci-cd
    docker network create projectnet
    # docker-compose -f docker-compose.yml up -d
  SHELL

end

