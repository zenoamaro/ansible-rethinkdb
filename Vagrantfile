# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure '2' do |config|


# RethinkDB standalone box
# -------------------------

	# Provision this machine to obtain a standalone box
	# with a RethinkDB inside listening on the default port.

	config.vm.define 'boxed' do |box|
		box.vm.box = "ubuntu/trusty64"
		# Configure the network topology to your needs
		config.vm.network :private_network, ip: "192.168.33.10"
		# config.vm.network :public_network
		config.vm.provision :ansible do |ansible|
			ansible.playbook       = './boxed.yml'
			ansible.inventory_path = './inventory'
		end
		# Forward admin and api ports on the host.
		config.vm.network :forwarded_port, guest: 29015, host: 29015 # cluster
		config.vm.network :forwarded_port, guest: 28015, host: 28015 # client
		config.vm.network :forwarded_port, guest: 8080,  host: 28080 # administrative
		# TODO: What about also forwarding the data storage?
	end


# Test machines
# -------------

	# These test machines will configure the installation with all
	# its extensions enabled, in order to test the validity
	# of the role.

	# Ubuntu machines are available:
	# - "test-ubuntu-precise"
	# - "test-ubuntu-trusty"

	def apply_test_ansible_defaults(ansible)
		ansible.playbook       = './test.yml'
		ansible.inventory_path = './inventory'
	end

	config.vm.define 'test-ubuntu-trusty', autostart:false do |box|
		box.vm.box = "ubuntu/trusty64"
		config.vm.network :private_network, ip: "192.168.33.21"
		config.vm.provision :ansible do |ansible|
			apply_test_ansible_defaults ansible
			ansible.extra_vars = {}
		end
	end

	config.vm.define 'test-ubuntu-precise', autostart:false do |box|
		box.vm.box = "ubuntu/precise64"
		config.vm.network :private_network, ip: "192.168.33.20"
		config.vm.provision :ansible do |ansible|
			apply_test_ansible_defaults ansible
			ansible.extra_vars = {}
		end
	end

end
