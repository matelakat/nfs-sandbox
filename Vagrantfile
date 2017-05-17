# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
common_script = <<SCRIPT
zypper ar http://192.168.0.200/repos/SLES12-SP1-Pool/ sles12sp1
zypper ar http://192.168.0.200/repos/SLES12-SP1-Updates/ sles12sp1up
zypper -qn up
SCRIPT

server_script = common_script + <<SCRIPT
zypper -qn in nfs-kernel-server
mkdir -p /share
cat - > /etc/exports << EOF
/share *(rw,sync,fsid=0,crossmnt,no_subtree_check)
EOF
rcnfsserver restart
rcrpcbind restart
SCRIPT

client_script = common_script + <<SCRIPT
zypper -qn in nfs-client
mount -t nfs -o proto=tcp,port=2049 192.168.111.11:/share /mnt
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.box = "suse/sles12sp1"
  config.vm.box_check_update = false

  config.vm.define "server" do |server|
      server.vm.network "public_network", ip: "192.168.111.11", bridge: "pubbr"

      server.vm.provider "virtualbox" do |vb|
        vb.gui = false
        vb.name = "server"
        vb.memory = 1024
        vb.cpus = 1
     end

     server.vm.provision "shell", inline: server_script
  end

  config.vm.define "client" do |client|
      client.vm.network "public_network", ip: "192.168.111.10", bridge: "pubbr"

      client.vm.provider "virtualbox" do |vb|
        vb.gui = false
        vb.name = "client"
        vb.memory = 1024
        vb.cpus = 1
     end

     client.vm.provision "shell", inline: client_script
  end
end
