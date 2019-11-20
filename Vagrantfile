# -*- mode: ruby -*-
# vim: set ft=ruby :


home = ENV['HOME']
ENV["LC_ALL"] = "en_US.UTF-8"
SATA = "SATA_controller"

MACHINES = {
    :lvm => {
        :box_name => "centos/7",
        :box_version => "1804.02",
        :ip_addr => '192.168.11.101',
        :disks => {
            :sata1 => {
                :dfile => home + '/VirtualBox VMs/sata1.vdi',
                :size => 10240,
                :port => 1
            },
            :sata2 => {
                :dfile => home + '/VirtualBox VMs/sata2.vdi',
                :size => 2048, # Megabytes
                :port => 2
            },
            :sata3 => {
                :dfile => home + '/VirtualBox VMs/sata3.vdi',
                :size => 1024, # Megabytes
                :port => 3
            },
            :sata4 => {
                :dfile => home + '/VirtualBox VMs/sata4.vdi',
                :size => 1024,
                :port => 4
            }
        }
    }
}

Vagrant.configure("2") do |config|

    config.vm.box_version = "1804.02"

    MACHINES.each do |boxname, boxconfig|
  
        config.vm.define boxname do |box|
  
            box.vm.box = boxconfig[:box_name]
            box.vm.host_name = boxname.to_s
            box.vm.network "private_network", ip: boxconfig[:ip_addr]

            box.vm.provider :virtualbox do |vb|
                vb.customize ["modifyvm", :id, "--memory", "256"]
                needsController = false
                boxconfig[:disks].each do |dname, dconf|
                unless File.exist?(dconf[:dfile])
                    vb.customize ['createhd', '--filename', dconf[:dfile], '--variant', 'Fixed', '--size', dconf[:size]]
                    needsController =  true
                end
            end

            if needsController == true
                vb.customize ["storagectl", :id, "--name", SATA, "--add", "sata" ]
                boxconfig[:disks].each do |dname, dconf|
                    vb.customize ['storageattach', :id,  '--storagectl', SATA, '--port', dconf[:port], '--device', 0, '--type', 'hdd', '--medium', dconf[:dfile]]
                end
            end
        end
  
        box.vm.provision "shell", inline: <<-SHELL
            mkdir -p ~root/.ssh
            cp ~vagrant/.ssh/auth* ~root/.ssh
            yum install -y nano mdadm smartmontools hdparm gdisk dmraid
            
            # Rename Volume Group
            NEW_VG="OtusRoot"
            OLD_VG="VolGroup00"
            vgrename $OLD_VG $NEW_VG
            sed -i "s/$OLD_VG/$NEW_VG/g" /etc/fstab /etc/default/grub /boot/grub2/grub.cfg
            sed -i -e 's/rhgb quiet//g' /boot/grub2/grub.cfg

            # Add a test module to initrd
            mkdir /usr/lib/dracut/modules.d/01test
            cp /vagrant/module-setup.sh /vagrant/test.sh /usr/lib/dracut/modules.d/01test
            chmod +x /usr/lib/dracut/modules.d/01test/*.sh
            dracut -f -v
        SHELL

        end
    end
end