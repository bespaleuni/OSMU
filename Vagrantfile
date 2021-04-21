  $configuration = <<SCRIPT
            docker pull ubuntu
            docker run --name ubuntuContainer -it ubuntu /bin/bash
            if lvs | grep vps; then
                echo "==> SHELL PROVISIONER: Setting up logical volumes"
                lvremove -f vg1
                sudo lvcreate -L 100M -n disk1 local
                sudo lvcreate -L 100M -n disk2 local
                sudo lvcreate -L 100M -n disk3 local
                sudo lvcreate -L 100M -n disk4 local
                sudo mkfs.ext4 /dev/local/disk1
                sudo mkfs.ext4 /dev/local/disk2
                sudo mkfs.ext4 /dev/local/disk3
                sudo mkfs.ext4 /dev/local/disk4
                sudo -s
                sudo apt-get install mdadm
                sudo mdadm --create /dev/md0 --level=mirror --raid-devices=2 /dev/local/disk1 /dev/local/disk2
                sudo mkfs.ext4 /dev/md0
                sudo mkdir /mnt/raid1
                sudo mount /dev/md0 /mnt/raid1
                sudo mdadm --create /dev/md1 --level=mirror --raid-devices=2 /dev/local/disk3 /dev/local/disk4
                sudo mkfs.ext4 /dev/md1
                sudo mkdir /mnt/raid2
                sudo mount /dev/md0 /mnt/raid2
                mount -a
                sudo visudo -f /etc/sudoers
                %sudo   ALL=(ALL:ALL)NOPASSWD:ALL
                sudo groupadd sampleGroup
                sudo adduser -m NameSurname
                sudo usermod -aG sampleGroup,daemon NameSurname -d /etc/
                sudo delgroup sampleGroup
            SCRIPT
Vagrant.configure("2") do |config|
        config.vm.box = "ubuntu/trusty64"
        config.vm.box_check_update = false
        config.vm.define "Kuzmiantsou-Ubuntu1" do |Kuzmiantsou-Ubuntu1|
            Kuzmiantsou-Ubuntu1.vm.network  "public_network", ip: "192.168.1.1"
            Kuzmiantsou-Ubuntu1.vm.hostname = "Kuzmiantsou-Ubuntu1"
            Kuzmiantsou-Ubuntu1.vm.provider "virtualbox" do |vb|
              vb.memory=1536
              vb.cpus=1
            end
            ubuntu.vm.provision "shell",
              inline: "
              sudo apt-get update && sudo apt-get upgrade
              sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
              curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
              sudo add-apt-repository 'deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable'
              sudo apt-get update && apt-cache policy docker-ce
              sudo apt-get install -y docker-ce
              "
            Kuzmiantsou-Ubuntu1.persistent_storage.enabled = true
            Kuzmiantsou-Ubuntu1.persistent_storage.location = "disks/sourcehdd.vdi"
            Kuzmiantsou-Ubuntu1.persistent_storage.size = 500
            Kuzmiantsou-Ubuntu1.persistent_storage.mount = false
            Kuzmiantsou-Ubuntu1.persistent_storage.use_lvm = true
            Kuzmiantsou-Ubuntu1.persistent_storage.format = false
            Kuzmiantsou-Ubuntu1.persistent_storage.volgroupname = 'vg1'
        end
        config.vm.define "Kuzmiantsou-Ubuntu2" do |Kuzmiantsou-Ubuntu2|
           Kuzmiantsou-Ubuntu2.vm.network "public_network", ip: "192.168.1.2"
           Kuzmiantsou-Ubuntu2.vm.hostname = "Kuzmiantsou-Ubuntu2"
           Kuzmiantsou-Ubuntu2.vm.provider "virtualbox" do |vb|
              vb.memory=1536
              vb.cpus=1
           end
            ubuntu.vm.provision "shell",
              inline: "
              sudo apt-get update && sudo apt-get upgrade
              sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
              curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
              sudo add-apt-repository 'deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable'
              sudo apt-get update && apt-cache policy docker-ce
              sudo apt-get install -y docker-ce
              "
            Kuzmiantsou-Ubuntu2.persistent_storage.enabled = true
            Kuzmiantsou-Ubuntu2.persistent_storage.location = "disks/sourcehdd1.vdi"
            Kuzmiantsou-Ubuntu2.persistent_storage.size = 500
            Kuzmiantsou-Ubuntu2.persistent_storage.mount = false
            Kuzmiantsou-Ubuntu2.persistent_storage.use_lvm = true
            Kuzmiantsou-Ubuntu2.persistent_storage.format = false
            Kuzmiantsou-Ubuntu2.persistent_storage.volgroupname = 'vg1'
        end
        config.vm.provision "shell", inline: $configuration
end