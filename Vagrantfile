# -*- mode: ruby -*-
# vi: set ft=ruby :

BOX_MEM = ENV['BOX_MEM'] || "1536"
BOX_NAME =  ENV['BOX_NAME'] || "ubuntu/trusty64"
BOX_URI = ENV['BOX_URI'] || "https://vagrantcloud.com/ubuntu/boxes/trusty64/versions/14.04/providers/virtualbox.box"

HOST_TLD = "LOCAL"

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

nodes = [
	{:name => "vps1", :cpu => 1, :mem => 1024, :ip => "10.1.42.10", :ssh_port => 2210, :http_port => 8180, :other_port => 5199},
	{:name => "vps2", :cpu => 1, :mem => 1024, :ip => "10.1.42.20", :ssh_port => 2220, :http_port => 8280, :other_port => 5299},
	{:name => "vps3", :cpu => 1, :mem => 1024, :ip => "10.1.42.30", :ssh_port => 2230, :http_port => 8380, :other_port => 5399}
]

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

	config.vm.box = BOX_NAME
	config.vm.box_url = BOX_URI

	config.ssh.forward_agent = true

	id_rsa_ssh_key_pub = File.read(File.expand_path('~') + '/.ssh/id_rsa.pub');

	nodes.each_with_index do |opts, index|
		config.vm.define opts[:name] do |config|
            config.vm.hostname = "%s.%s" % [opts[:name].to_s, HOST_TLD]
            config.vm.network :private_network, ip: opts[:ip]

            config.vm.provision :shell, :inline => "echo 'Copying local id_rsa SSH Key to VM auth_keys for auth purposes (login into VM included)...' && echo '#{id_rsa_ssh_key_pub }' >> /home/vagrant/.ssh/authorized_keys && chmod 600 /home/vagrant/.ssh/authorized_keys"
            config.vm.network :forwarded_port, guest: 80, host: opts[:http_port]
            config.vm.network :forwarded_port, guest: 5099, host: opts[:other_port]
            config.vm.network :forwarded_port, guest: 22, host: opts[:ssh_port], id: "ssh"

            config.vm.provider "virtualbox" do |v|
              v.name = opts[:name]
              v.customize ["modifyvm", :id, "--memory", BOX_MEM]
              v.customize ["modifyvm", :id, "--ioapic", "on"]
              v.customize ["modifyvm", :id, "--cpus", opts[:cpu]]
              v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
              v.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
            end

            config.vm.provision :shell, inline: 'echo A'

#             config.vm.provision :hosts do |provisioner|
#                 provisioner.autoconfigure = false
#                 provisioner.add_localhost_hostnames = false
#                 nodes.each do |n|
#                     provisioner.add_host n[:ip], [ "%s.%s" % [ n[:name].to_s, HOST_TLD ], n[:name].to_s ]
#                 end
#             end

                # provision nodes with ansible
            if index == nodes.size - 1
                config.vm.provision :ansible do |ansible|
                    ansible.playbook = "playbook.yml"
                    ansible.inventory_path = "vagrant_inventory"
                    ansible.verbose = "vvvv"
                    ansible.limit = "all"
                    ansible.extra_vars = {
                        ntp_server: "fr.pool.ntp.org",
                    }
                    ansible.groups = {
                        "mysql_master" => ["vps1"],
                        "mysql_slave" => ["vps2", "vps3"],
                        "commute" => ["mysql_master", "mysql_slave"]
                    }
                end

            end

        end

	end

end