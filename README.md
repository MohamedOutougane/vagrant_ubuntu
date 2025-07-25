# vagrant_ubuntu
Script to create vms with vagrant

## How to Use This Repository

If you want to use this repository to create your own Ubuntu VM with Xubuntu desktop, follow these simple steps:

### Prerequisites
- Install Vagrant on your system: [https://developer.hashicorp.com/vagrant/install](https://developer.hashicorp.com/vagrant/install)
- Install VirtualBox: [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
- Restart your computer after installation

### Steps to Create the VM

1. **Clone this repository**
   ```bash
   git clone https://github.com/MohamedOutougane/vagrant_ubuntu.git
   cd vagrant_ubuntu
   ```

2. **Start the virtual machine**
   ```bash
   vagrant up
   ```
   
   This command will:
   - Download the Ubuntu Jammy box (first time only)
   - Create and configure the VM
   - Install Xubuntu desktop environment
   - Configure French keyboard layout
   - Install VirtualBox Guest Additions
   - Reboot the VM

3. **Access your VM**
   - The VM will open with a graphical interface
   - Login with username: `vagrant` and password: `vagrant`
   - Enjoy your Ubuntu desktop environment!

### Useful Commands
- `vagrant halt` - Stop the VM
- `vagrant destroy` - Delete the VM completely
- `vagrant ssh` - Connect to the VM via SSH
- `vagrant reload` - Restart the VM

---


## Project Development Process
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

### 2. Boot Timeout Configuration
```ruby
config.vm.boot_timeout = 900
```
Set the boot timeout to 15 minutes (900 seconds) to allow for slower systems and desktop environment installation.

### 3. VirtualBox Provider Configuration
```ruby
config.vm.provider "virtualbox" do |vb|
  vb.gui = true
  vb.memory = "2048"
  vb.cpus = 2
  vb.customize ["modifyvm", :id, "--graphicscontroller", "vmsvga"]
end
```
- **GUI enabled**: `vb.gui = true` displays the VirtualBox interface when starting the VM
- **Memory allocation**: Set to 2048 MB (2 GB) for better performance
- **CPU cores**: Allocated 2 CPU cores to the VM

### 4. Enhanced Provisioning Script
```ruby
config.vm.provision "shell", privileged: true, inline: <<-SHELL
  # System updates
  apt-get update
  apt-get upgrade -y
  
  # Install Xubuntu desktop environment
  DEBIAN_FRONTEND=noninteractive apt-get install -y xubuntu-desktop lightdm
  
  # Configure display manager and graphical target
  echo "set shared/default-x-display-manager lightdm" | debconf-set-selections
  systemctl set-default graphical.target
  
  # Set French keyboard layout
  echo 'XKBLAYOUT="fr"' > /etc/default/keyboard
  dpkg-reconfigure -f noninteractive keyboard-configuration
  
  # Install VirtualBox Guest Additions
  apt-get install -y build-essential dkms linux-headers-$(uname -r)
  apt-get install -y virtualbox-guest-dkms virtualbox-guest-utils virtualbox-guest-x11
  
  # Enable VBoxService
  systemctl enable vboxservice
  systemctl start vboxservice
  
  reboot
SHELL
```

## Issues Encountered and Solutions

### Initial Setup Problems

During my first setup attempt, I encountered several issues that required modifications to the Vagrantfile:

1. **Keyboard Layout Issue**: The VM was configured with QWERTY layout by default, but I needed AZERTY (French) layout
2. **Graphical Interface Problems**: The initial Ubuntu Desktop installation didn't work properly and the GUI wasn't displaying correctly
3. **Mouse and Graphics Controller Issues**: Even after switching to Xubuntu, I still had problems with mouse responsiveness and broken graphical interface

### Solutions Implemented

To resolve these issues, I made the following changes:

- **Switched to Xubuntu**: Replaced the heavy Ubuntu Desktop with the lighter Xubuntu desktop environment for better performance and compatibility
- **Added French Keyboard Support**: Configured the system to use French (AZERTY) keyboard layout automatically
- **Enhanced VirtualBox Integration**: Added proper VirtualBox Guest Additions installation for better host-guest integration
- **Improved Display Management**: Configured LightDM as the display manager and set the system to boot into graphical mode
- **Non-interactive Installation**: Used `DEBIAN_FRONTEND=noninteractive` to prevent installation prompts that could hang the provisioning process
- **Graphics Controller Fix**: Changed the graphics controller from the default VBoxVGA to VMSVGA for better compatibility with modern desktop environments:

```ruby
vb.customize ["modifyvm", :id, "--graphicscontroller", "vmsvga"]
```

This final modification resolved all the mouse and graphical interface issues I was experiencing. With VMSVGA as the graphics controller, everything now works correctly.

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
- Installs Xubuntu desktop environment with French keyboard layout
- Automatically reboots to apply all changes
- Displays the Xubuntu login screen with full graphical interface

The VM now boots successfully into a fully functional Xubuntu desktop environment with French keyboard layout and proper VirtualBox integration.

## Vagrant Directory Management and GitIgnore

When the virtual machine was launched, I noticed that Vagrant automatically generated a `.vagrant` folder containing various subdirectories and files that appear to contain sensitive information such as machine IDs and provider-specific configurations. To prevent accidentally pushing these confidential files to the repository, I created a `.gitignore` file to exclude the `.vagrant` directory from version control.


---
Last update : 19-07-2025