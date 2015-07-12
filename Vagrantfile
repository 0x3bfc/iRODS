# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

	common_virtualbox = Proc.new do |virtualbox, override|
		config.vm.box = "ubuntu/precise64"
		config.vm.hostname = "master"
		config.ssh.username = "vagrant"
		config.ssh.password = "vagrant"
		config.ssh.insert_key = 'true'
	end

	config.vm.define 'master' do |cfg|
		cfg.vm.provider :virtualbox do |virtualbox, override|
			common_virtualbox.call virtualbox, override
			cfg.vm.hostname = "master"
			cfg.vm.network :private_network, ip: "192.168.0.20"
			virtualbox.customize ["modifyvm", :id,"--memory", "1024"]
		end
	end
       config.vm.define 'slave1' do |cfg|
                cfg.vm.provider :virtualbox do |virtualbox, override|
                        common_virtualbox.call virtualbox, override
                        cfg.vm.hostname = "slave1"
			cfg.vm.network :private_network, ip: "192.168.0.21"
                        virtualbox.customize ["modifyvm", :id,"--memory", "1024"]
                end
        end
       config.vm.define 'slave2' do |cfg|
                cfg.vm.provider :virtualbox do |virtualbox, override|
                        common_virtualbox.call virtualbox, override
                        cfg.vm.hostname = "slave2"
                        cfg.vm.network :private_network, ip: "192.168.0.22"
                        virtualbox.customize ["modifyvm", :id,"--memory", "1024"]
                end
        end

end
