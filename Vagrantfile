$script = <<-'SCRIPT'
sudo apt-get update && upgrade
sudo apt-get install git -y
git clone -b lab-2 https://github.com/bespaleuni/OSMU.git
cat ~/OSMU/file.txt
SCRIPT

Vagrant.configure("2") do |config|
        config.vm.define "Ubuntu1" do |conf|
          conf.vm.box = "ubuntu/trusty64"
          conf.vm.hostname = "Kuzmiantsou-Ubuntu1"
          conf.vm.network :private_network, ip: "192.168.10.12"

          conf.vm.provider :virtualbox do |vb|
              vb.customize [
                  "modifyvm", :id,
                  "--memory", "1536",
                  "--cpus", "1"
              ]
          end

          conf.vm.provision "shell", inline: $script
end

        config.vm.define "Ubuntu2" do |conf|
          conf.vm.box = "ubuntu/trusty64"
          conf.vm.hostname = "Kuzmiantsou-Ubuntu2"
          conf.vm.network :private_network, ip: "192.168.10.13"

          conf.vm.provider :virtualbox do |vb|
              vb.customize [
                  "modifyvm", :id,
                  "--memory", "1536",
                  "--cpus", "1"
              ]
          end

          conf.vm.provision "shell", inline: $script
      end
  end