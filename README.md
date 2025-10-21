Markdown
# Vagrant and VMware Fusion Setup Guide for Apple Silicon (ARM64)

This guide details the steps required to configure **Vagrant** with **VMware Fusion** on Apple Silicon Macs (M1/M2/M3, etc.). The goal is to run ARM64-based virtual machines (VMs), such as Ubuntu and CentOS Stream.

## Prerequisites

* An Apple Silicon-based Mac.
* [Homebrew](https://brew.sh/) installed.
* Basic knowledge of the terminal.
* Administrator privileges on your Mac.

---

## Installation Steps

### 1. Install Rosetta 2

Rosetta 2 is necessary to run some x86_64 binaries that may be part of the tooling.

Open your terminal and execute the command:
```bash
/usr/sbin/softwareupdate --install-rosetta --agree-to-license
2. Install Vagrant

Use Homebrew to install Vagrant:

Bash
brew install vagrant
3. Download & Install VMware Fusion

A VMware Fusion Pro license is required for the Vagrant plugin. A free Personal Use license is available.

Create a Broadcom Account: Go to https://support.broadcom.com and click "Register".

Login to your account.

Navigate to Downloads:

Click your username in the top-right corner and select VMware Cloud Foundation.

Click My Downloads.

Search for or select VMware Fusion.

Select Version:

Select VMware Fusion 13 Pro for Personal Use.

Select version 13.6 (or the latest compatible version).

Download and Install:

Click the download icon.

Once the .dmg file is downloaded, open it.

Double-click the VMware Fusion icon to begin the installation and enter your system password when prompted.

4. Grant Accessibility Permissions

VMware Fusion requires accessibility permissions for Vagrant to properly manage VMs.

Open System Settings.

Go to Privacy & Security > Accessibility.

Find VMware Fusion in the list and toggle the switch on. You may need to unlock the settings with your password first.

5. Install Vagrant VMware Utility

This is a helper utility required by the Vagrant plugin.

Bash
brew install --cask vagrant-vmware-utility
6. Install Vagrant VMware Plugin

This plugin connects Vagrant to VMware Fusion.

Bash
vagrant plugin install vagrant-vmware-desktop
Virtual Machine Setup
Project 1: Ubuntu ARM

Create the project directory:

Bash
cd
mkdir -p Desktop/vms/ubuntu
cd Desktop/vms/ubuntu
Create the Vagrantfile: Create a new file named Vagrantfile (e.g., using vim Vagrantfile or nano Vagrantfile) and paste the following content:

Ruby
Vagrant.configure("2") do |config|
  config.vm.box = "spox/ubuntu-arm"
  config.vm.box_version = "1.0.0"
  config.vm.network "private_network", ip: "192.168.56.11"

  config.vm.provider "vmware_desktop" do |vmware|
    vmware.gui = true
    vmware.allowlist_verified = true
  end
end
Manage the VM: Ensure you are in the Desktop/vms/ubuntu directory in your terminal.

Bash
# Bring up the virtual machine
vagrant up

# Connect to the machine via SSH
vagrant ssh

# Inside the VM: get root privileges and check IP
sudo -i
ip addr show
exit
exit

# --- VM Lifecycle Management ---

# To stop the machine:
# vagrant halt

# To delete the machine:
# vagrant destroy
Project 2: CentOS Stream 9 ARM

Create the project directory:

Bash
cd
mkdir -p Desktop/vms/centos
cd Desktop/vms/centos
Create the Vagrantfile: Create a new Vagrantfile in this directory and paste the following content:

Ruby
Vagrant.configure("2") do |config|
  config.vm.box = "bandit145/centos_stream9_arm"
  config.vm.network "private_network", ip: "192.168.56.12"

  config.vm.provider "vmware_desktop" do |vmware|
    vmware.gui = true
    vmware.allowlist_verified = true
  end
end
Manage the VM: Ensure you are in the Desktop/vms/centos directory in your terminal.

Bash
# Bring up the virtual machine
vagrant up

# Connect to the machine via SSH
vagrant ssh

# Inside the VM: get root privileges and check IP
sudo -i
ip addr show
exit
exit

# --- VM Lifecycle Management ---

# To stop the machine:
# vagrant halt

# To delete the machine:
# vagrant destroy
