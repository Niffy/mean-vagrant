# -*- mode: ruby -*-
# vi: set ft=ruby :

require "yaml"

_config = YAML.load(File.open(File.join(File.dirname(__FILE__), "/yaml/vagrantconfig.yaml"), File::RDONLY).read)

CONF = _config

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
	config.vm.define :mongodb do |mongodb|
    	mongodb.vm.box = "ubuntu_precise32"
		mongodb.vm.box_url = "http://files.vagrantup.com/precise32.box"
		mongodb.vm.network "public_network", ip: "192.168.1.100"

		# This shell provisioner installs librarian-puppet and runs it to install
  		# puppet modules. This has to be done before the puppet provisioning so that
  		# the modules are available when puppet tries to parse its manifests.
		# must pass in directory of the Puppetfile location
		mongodb.vm.provision :shell, :path => "shell/prereqs.sh", :args => "puppet/librarian/mongodb"

		# Now run the puppet provisioner. Note that the modules directory is entirely
		# managed by librarian-puppet
    		mongodb.vm.provision :puppet do |mongodb_puppet|
    			mongodb_puppet.manifests_path = "puppet/manifests"
    			mongodb_puppet.manifest_file  = "mongo.pp"
		end
  	end	
	config.vm.define :appserver do |appserver|
    	appserver.vm.box = "ubuntu_precise32"
		appserver.vm.box_url = "http://files.vagrantup.com/precise32.box"
		appserver.vm.network "public_network", ip: "192.168.1.101"
	
		# This shell provisioner installs librarian-puppet and runs it to install
  		# puppet modules. This has to be done before the puppet provisioning so that
  		# the modules are available when puppet tries to parse its manifests.
		# must pass in directory of the Puppetfile location
		appserver.vm.provision :shell, :path => "shell/prereqs.sh", :args => "puppet/librarian/appserver"

		# Now run the puppet provisioner. Note that the modules directory is entirely
		# managed by librarian-puppet
    		appserver.vm.provision :puppet do |appserver_puppet|
    			appserver_puppet.manifests_path = "puppet/manifests"
    			appserver_puppet.manifest_file  = "appserver.pp"
		end
		# define the name of your application here within the args e.g replace "mytestapp" with your app name
		appserver.vm.provision :shell, :path => "shell/kickstart-app.sh",:args => "mytestapp"

  	end
end
