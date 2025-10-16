# Vagrant & VMware Devlopment Environment on Apple Silicon Macs

This guide provides step-by-step instructions for setting up a complete local development environment using Vagrant and VMware Fusion on an Apple Silicon (M1/M2/M3) Mac.

## 📋 Prerequisites

Before you begin, ensure you have the following:
*   An Apple Silicon Mac (M1, M2, M3, etc.).
*   [Homebrew](https://brew.sh/) installed.
*   Administrator privileges on your machine.

---

## ⚙️ Part 1: One-Time Environment Setup

Follow these steps to install and configure all the necessary software. This only needs to be done once.

### 1. Install Rosetta 2

Rosetta is required to run some Intel-based tools on Apple Silicon.

```bash
/usr/sbin/softwareupdate --install-rosetta --agree-to-license
```

### 2. Install Vagrant

Use Homebrew to install the Vagrant CLI.

```bash
brew install vagrant
```

### 3. Get VMware Fusion Pro for Personal Use

VMware Fusion is required as the virtualization provider. You can get a free license for personal use.

1.  **Create a Broadcom Account**: Go to [https://support.broadcom.com](https://support.broadcom.com) and click **Register**.
2.  **Navigate to Downloads**:
    *   After logging in, click your profile in the top-right corner and select **VMware Cloud Foundation**.
    *   Click **My Downloads** in the new menu.
    *   Search for or select **VMware Fusion**.
3.  **Download the Software**:
    *   Select the product **VMware Fusion 13 Pro for Personal Use**.
    *   Choose the latest version (e.g., `13.x.x`).
    *   Click the **Download** icon to get the installer.
4.  **Install VMware Fusion**:
    *   Double-click the downloaded `.dmg` file.
    *   Double-click the VMware Fusion icon in the window that appears and follow the installation prompts, entering your password when required.

### 4. Grant Accessibility Permissions

VMware Fusion needs system permissions to function correctly.

1.  Open **System Settings**.
2.  Go to **Privacy & Security** -> **Accessibility**.
3.  Find **VMware Fusion** in the list and toggle the switch **on**. You may need to unlock with your password.

### 5. Install Vagrant VMware Utility

This utility allows Vagrant to communicate with VMware Fusion.

```bash
brew install --cask vagrant-vmware-utility
```

### 6. Install Vagrant VMware Plugin

This is the core plugin that integrates Vagrant with the VMware provider.

```bash
vagrant plugin install vagrant-vmware-desktop
```

> ✅ **Setup Complete!** Your machine is now ready to run Vagrant-managed virtual machines with VMware.

---

## 🚀 Part 2: Creating and Managing a VM

Here’s how to create and manage your virtual machines.

### Step 1: Create a Project Directory

It's best practice to create a separate directory for each virtual machine.

```bash
# Example for an Ubuntu VM
mkdir -p ~/Desktop/vms/ubuntu
cd ~/Desktop/vms/ubuntu
```

### Step 2: Create a `Vagrantfile`

This file defines your virtual machine's configuration. Create a file named `Vagrantfile` inside your project directory.

```bash
# You can use any text editor, `vim` is just an example
vim Vagrantfile
```

### Step 3: Add Configuration to `Vagrantfile`

Copy and paste one of the configurations below into your `Vagrantfile`.

---

#### Example A: Ubuntu (ARM)

This will set up an Ubuntu VM compatible with Apple Silicon.

```ruby
# Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.box = "spox/ubuntu-arm"
  config.vm.box_version = "1.0.0"

  config.vm.network "private_network", ip: "192.168.56.11"

  config.vm.provider "vmware_desktop" do |vmware|
    vmware.gui = true
    vmware.allowlist_verified = true
  end
end
```

---

#### Example B: CentOS Stream 9 (ARM)

This will set up a CentOS Stream 9 VM compatible with Apple Silicon.

```bash
# First, create the directory
mkdir -p ~/Desktop/vms/centos
cd ~/Desktop/vms/centos
# Then create the Vagrantfile with the content below
```

```ruby
# Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.box = "bandit145/centos-stream9-arm"

  config.vm.network "private_network", ip: "192.168.56.12"

  config.vm.provider "vmware_desktop" do |vmware|
    vmware.gui = true
    vmware.allowlist_verified = true
  end
end
```

---

## ✨ Part 3: VM Lifecycle Commands

Run these commands from your terminal, inside the directory containing your `Vagrantfile` (e.g., `~/Desktop/vms/ubuntu`).

| Command         | Description                                               |
| --------------- | --------------------------------------------------------- |
| `vagrant up`      | Starts and provisions the virtual machine.                |
| `vagrant ssh`     | Connects to the running machine via SSH.                  |
| `vagrant halt`    | Shuts down the virtual machine gracefully.                |
| `vagrant suspend` | Pauses the virtual machine, saving its current state.     |
| `vagrant resume`  | Resumes a suspended virtual machine.                      |
| `vagrant destroy` | **Deletes the virtual machine and all its data.** Use with caution. |

### Typical Workflow Example

```bash
# 1. Start the machine
vagrant up

# 2. Connect to it
vagrant ssh

# Inside the VM, you can check its IP and work as root
# sudo -i
# ip addr show
# exit
# exit

# 3. When you're done for the day, shut it down
vagrant halt
```
