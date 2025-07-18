# vagrant_ubuntu
Script to create vms with vagrant

## Project Objective

This project aims to learn how to create and manage virtual machines with Vagrant. It's a hands-on approach to understand virtualization concepts and infrastructure as code.

## Setup Steps

### 1. Installing Vagrant on Windows

The first step is to install Vagrant on Windows. You can download the installer from the official website: [https://developer.hashicorp.com/vagrant/install](https://developer.hashicorp.com/vagrant/install)

### 2. System Restart

After installing Vagrant, it's necessary to restart the PC so that environment variables are properly configured and Vagrant is accessible from the terminal.

### 3. Vagrant Project Initialization

Once Vagrant was installed and the system restarted, I executed the following command in my directory to initialize the project:

```bash
cd vagrant_ubuntu
vagrant init ubuntu/jammy64
```

This command generates a basic `Vagrantfile` configured to use the Ubuntu image. This file contains the default configuration that I will modify according to my specific needs.

## Vagrantfile Modifications

I made several modifications to the generated Vagrantfile to customize the VM according to my learning objectives:

### 1. Network Configuration
```ruby
config.vm.network "private_network", ip: "192.168.33.10"
```
I enabled a private network with a static IP address to allow direct communication between the host and guest machine.

### 2. VirtualBox Provider Configuration
```ruby
config.vm.provider "virtualbox" do |vb|
  vb.gui = true
  vb.memory = "2048"
  vb.cpus = 2
end
```
- **GUI enabled**: `vb.gui = true` displays the VirtualBox interface when starting the VM
- **Memory allocation**: Set to 2048 MB (2 GB) for better performance
- **CPU cores**: Allocated 2 CPU cores to the VM

### 3. Provisioning Script
```ruby
config.vm.provision "shell", inline: <<-SHELL
  apt-get update
  apt-get install -y apache2
  apt install -y ubuntu-desktop
SHELL
```
Added automatic provisioning to:
- Update the package list
- Install Apache2 web server
- Install Ubuntu Desktop environment (GUI)

## Starting the Virtual Machine

After these modifications, when I run:

```bash
vagrant up
```

The command launches a VM on VirtualBox with the following behavior:
- Downloads the Ubuntu Jammy box
- Creates and configures the VM with the specified settings
- Starts the VM without GUI enabled
- Runs the provisioning script to install software
- Displays the Ubuntu login screen where I need to authenticate

The VM boots to an Ubuntu Jammy login in a tty interface where I can log.

## Vagrant Directory Management and GitIgnore

When the virtual machine was launched, I noticed that Vagrant automatically generated a `.vagrant` folder containing various subdirectories and files that appear to contain sensitive information such as machine IDs and provider-specific configurations. To prevent accidentally pushing these confidential files to the repository, I created a `.gitignore` file to exclude the `.vagrant` directory from version control.


---
Last update : 18-07-2025