# -*- mode: ruby -*-
# vi: set ft=ruby :

$pre_provision_update = <<-SCRIPT
sudo su
sudo DEBIAN_FRONTEND=noninteractive apt-get -o Dpkg::Options::='--force-confold' --allow-unauthenticated --allow-downgrades --allow-remove-essential --allow-change-held-packages -uy update
sudo DEBIAN_FRONTEND=noninteractive apt-get -o Dpkg::Options::='--force-confold' --allow-unauthenticated --allow-downgrades --allow-remove-essential --allow-change-held-packages -fuy dist-upgrade
SCRIPT

Vagrant.configure("2") do |config|

    config.vm.box = "bento/ubuntu-18.04"
    config.vm.network "private_network", ip: "192.168.33.10"

    # ? Uncomment lines below and use your path context in order to mount host folder (your current dev folder) to the guest (Vagrant VM) automatically
    # current_local_dev_path="/Users/kostiantynsharokhin/Downloads/development/custom/plugins"
    # vm_folder_path="/home/vagrant/shopware-dev/custom/plugins"

    # config.vm.synced_folder current_local_dev_path, vm_folder_path, 
    #    type: "sshfs",
    #    sshfs_opts_append: "-o nonempty"
    #

    config.ssh.insert_key = false

    config.vm.hostname = "shopware.local"
    config.vm.provider :virtualbox do |v|
        v.name = "shopware.local"
        v.memory = "4096"
        v.cpus = 2
        v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        v.customize ["modifyvm", :id, "--ioapic", "on"]
    end

    # Force a pre provision update
    config.vm.provision 'shell', keep_color: true, inline: $pre_provision_update

    config.vm.provision "ansible_local" do |ansible|
        ansible.compatibility_mode = "2.0"
        ansible.playbook = "ansible/playbook.yml"
    end

end
