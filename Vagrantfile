 Vagrant.configure("2") do |config|
  
  
  servers=[
	{
	:hostname => "h1",
	:box => "ubuntu/trusty64",
	:ip => "192.168.10.11",
	:ssh_port => '2211'
	},
	{
	:hostname => "h2",
	:box => "ubuntu/trusty64",
	:ip => 192.168.10.12",
	:ssh_port => '2212'
	},
	{
	:hostname => "h3",
	:box => "ubuntu/trusty64",
	:ip => 192.168.10.13",
	:ssh_port => '2213'
	}
  ]
  
  
  servers.each do |machine|
		config.vm.define machine[:hostname] do |node|
			node.vm.box = machine[:box]
			node.vm.hostname = machine[:hostname]
			node.vm.network :private_network, ip: machine[:ip]
			node.vm.network "forwarded_port", guest: 22, host: machine[:ssh_port], id: "ssh"
			node.vm.synced_folder "c:/github/osmu", "/var/www"

			
			#SSH
			#ssh-keygen -b 4096
			#ssh-copy-id siroga@192.168.10.12
			#ssh siroga@192.168.10.12
			#sudo nano /etc/ssh/sshd_config
			#sudo systemctl restart ssh
			
			#chmod 777 file.txt
			#scp root@192.168.10.11:dirlist.txt 
			#tar -cf filename.tar file file1 fileN
			
			#mkdir current new old 
			#tar cf myarch.tar current new old 

			
			node.vm.provision "shell", 
				
			inline: '
			
			',
			run: "always",
			privileged: "false"

				
			node.vm.provider :virtualbox do |vb|
				vb.gui = false
				vb.memory = 1536
				vb.cpus = 2
				
			end
				
		end
	
	end
	
end