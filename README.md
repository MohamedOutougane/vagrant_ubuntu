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

## Next Steps

The generated `Vagrantfile` will be customized to:
- Configure VM resources (RAM, CPU)
- Define network settings
- Add provisioning scripts
- Configure shared


---
Last update : 18-07-2025