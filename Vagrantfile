# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_DEFAULT_PROVIDER'] = "virtualbox"

# VM PARAMETERS

VAGRANT_BOX = "ubuntu/jammy64"              # Vagrant box 
VBOX_DISPLAY_NAME = "ubuntu-22.04"          # VM name that you will see on virtualbox
VBOX_HOSTNAME = "ubuntu.homelab.com"        # Hostname mapped to linux VM
VBOX_RAM = 2500                             # VM ram size
VBOX_CORE = 2                               # VM Core size
VBOX_DISK_SIZE = "50GB"                     # VM Disk size

# Shell provisioner to setup necessary tools and settings in VM #

$ubuntu = <<-'UB1'

  echo "
      ____   ___   ___ _____ ____ _____ ____      _    ____  
     | __ ) / _ \ / _ \_   _/ ___|_   _|  _ \    / \  |  _ \ 
     |  _ \| | | | | | || | \___ \ | | | |_) |  / _ \ | |_) |
     | |_) | |_| | |_| || |  ___) || | |  _ <  / ___ \|  __/ 
     |____/ \___/ \___/ |_| |____/ |_| |_| \_\/_/   \_\_|    
                                                             
  "

  # Additional user other than default vagrant user
  # Password will be 'vagrant' for both custom user and vagrant user
  # Change the user name if required.
      
  CUSTOM_USERNAME="user1"
  echo "[USER ADDITION] Adding user ${CUSTOM_USERNAME}" && \
  sudo useradd -m -p $(openssl passwd -1 vagrant) -s /bin/bash -G sudo ${CUSTOM_USERNAME} && \
  sudo echo "${CUSTOM_USERNAME} ALL=(ALL) NOPASSWD: ALL" > "/etc/sudoers.d/${CUSTOM_USERNAME}-nopasswd"

  touch /home/vagrant/.hushlogin 
  touch /home/${CUSTOM_USERNAME}/.hushlogin

  # Necessary system tools
  sudo apt-get update && \
  sudo apt-get -y install neovim curl wget htop net-tools iputils-ping cowsay screenfetch sysstat 

  # Fish shell #
  sudo apt-get install fish -y

  # Python packages
  sudo apt-get install python3-virtualenv python3-pip bpython -y

  # Container #
  sudo apt-get install podman -y && podman run hello

  # Ansible Installation #
  pip install ansible

  # Git installation & basic configuration #
  sudo apt-get install git -y 
  # git config --global user.name ""               # Add your name and email before running the vagrant up command 
  # git config --global user.email ""
  # git config --global core.editor "nvim"
  # git config --global init.defaultBranch main

  # Disable UFW firewall, comment below line if you wish to enable UFW #
  which ufw && sudo ufw disable && sudo ufw status

  # post message #
  echo "-------------------------------------------"
  echo "REBOOT MACHINE                             "
  echo "vagrant reload                             "
  echo "-------------------------------------------"

UB1

Vagrant.configure("2") do |config|

  config.vm.box = VAGRANT_BOX
  config.vm.box_check_update = false
  config.vbguest.auto_update = false

  # Sync folder
  config.vm.synced_folder ".", "/vagrant", disabled: false     # Default synced folder
  # config.vm.synced_folder <custom-path>, <mount>              # Custom synced folder

  # Network Configuration #

  config.vm.hostname = VBOX_HOSTNAME
  # config.vm.network "private_network", IP: <ip-here>         # Add private IP address here

  # Storage Configuration #
  config.vm.disk :disk, size: VBOX_DISK_SIZE, primary: true

  # Virtualbox resource and folder Configuration #
  config.vm.provider "virtualbox" do |rs|
    rs.memory = VBOX_RAM
    rs.cpus = VBOX_CORE
    rs.customize ["modifyvm", :id, "--groups", "/SERVER"]
    rs.name = VBOX_DISPLAY_NAME
  end 

  # Shell provisioner #
  config.vm.provision "shell", inline: $ubuntu

end
