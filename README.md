````markdown
# Vagrant with VMware Fusion on macOS Apple Silicon

Quick setup guide for running ARM virtual machines on Apple Silicon Macs using Vagrant and VMware Fusion.

## Prerequisites

- macOS 13+ on Apple Silicon (M1/M2/M3)
- Admin access
- Homebrew installed

## Installation Steps

### 1. Install Rosetta

Required for x86_64 compatibility on Apple Silicon:

```bash
/usr/sbin/softwareupdate --install-rosetta --agree-to-license
```

### 2. Install Vagrant

```bash
brew install vagrant
```

### 3. Create Broadcom Account

Go to https://support.broadcom.com and register for an account. You'll need this to download VMware Fusion.

### 4. Download and Install VMware Fusion 13 Pro

1. Login to https://support.broadcom.com
2. Click on your profile (top right) and select "VMware Cloud Foundation"
3. Go to My Downloads
4. Select VMware Fusion
5. Choose "VMware Fusion 13 Pro for Personal Use"
6. Select version 13.6
7. Download and install

### 5. Grant macOS Permissions

Open System Settings > Privacy & Security > Accessibility and enable VMware Fusion.

### 6. Install VMware Utility

```bash
brew install --cask vagrant-vmware-utility
```

### 7. Install Vagrant VMware Plugin

```bash
vagrant plugin install vagrant-vmware-desktop
```

## Creating Virtual Machines

### Ubuntu ARM VM

Create project directory:
```bash
cd
mkdir -p Desktop/vms/ubuntu
cd Desktop/vms/ubuntu
```

Create Vagrantfile with this content:
```ruby
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

Start and test the VM:
```bash
vagrant up
vagrant ssh
sudo -i
ip addr show
exit
exit
vagrant halt
vagrant destroy
```

### CentOS Stream 9 ARM VM

Create project directory:
```bash
cd
mkdir -p Desktop/vms/centos
cd Desktop/vms/centos
```

Create Vagrantfile:
```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "bandit145/centos_stream9_arm"
  config.vm.network "private_network", ip: "192.168.56.12"
  
  config.vm.provider "vmware_desktop" do |vmware|
    vmware.gui = true
    vmware.allowlist_verified = true
  end
end
```

Start and test:
```bash
vagrant up
vagrant ssh
sudo -i
ip addr show
exit
exit
vagrant halt
vagrant destroy
```

## Common Commands

```bash
vagrant up        # start VM
vagrant ssh       # connect to VM
vagrant halt      # stop VM
vagrant reload    # restart VM
vagrant destroy   # delete VM
```

## Troubleshooting

**VMware provider not found**
- Run: `vagrant plugin install vagrant-vmware-desktop`
- Verify with: `vagrant plugin list`

**Network issues**
- Make sure vagrant-vmware-utility is running
- Try: `brew services restart vagrant-vmware-utility`

**Permission errors**
- Check System Settings > Privacy & Security > Accessibility
- VMware Fusion must be enabled

**IP conflicts**
- Change the IP address in Vagrantfile to something else in the 192.168.56.x range
- Run `vagrant reload` after changes

**Rosetta missing error**
- Install Rosetta: `/usr/sbin/softwareupdate --install-rosetta --agree-to-license`

## Notes

Default VM credentials are usually:
- Username: vagrant
- Password: vagrant

The setup uses private networking, so VMs can communicate with each other and the host but are isolated from external networks.
````
