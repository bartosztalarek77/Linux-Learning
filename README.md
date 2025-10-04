# Linux-Learning

VM Setup <br/>
01 VM on MACOS M1 Chip
<br />
Vagrant.configure("2") do |config|
config.vm.box = "spox/ubuntu-arm"
config.vm.box_version = "1.0.0"
config.vm.network "private_network", ip: "192.168.56.11"
config.vm.provider "vmware_desktop" do |vmware|
vmware.gui = true
vmware.allowlist_verified = true
end
end
9. Bring 
