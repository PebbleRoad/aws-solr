# -*- mode: ruby -*-
# vi: set ft=ruby :

PROJECT_NAME = "solr"
# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # define the box to be used
  config.vm.box     = "trusty64"

  config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"

  config.vm.provider "virtualbox" do |v|
      # name the running box
      v.name = PROJECT_NAME

	  # You can adjust this to the amount of CPUs your system has available
	  v.customize ["modifyvm", :id, "--cpus", "1"]

	  # set the memory usage
	  v.customize ["modifyvm", :id, "--memory", "1024"]
  end

  config.vm.network "forwarded_port", guest: 80, host: 4080
  config.vm.network "forwarded_port", guest: 443, host: 4443
  config.vm.network "private_network", ip: "192.168.2.10"
  # config.vm.network "public_network"

  # Set the share folder for web docroot
  # config.vm.synced_folder ".", "/var/www", :owner => 'vagrant', :group => 'www-data', :mount_options => ['dmode=775', 'fmode=775']

  # Set the guest host name
  config.vm.host_name = PROJECT_NAME

  # forward ssh keys through into the machine
  config.ssh.forward_agent = true

  config.vm.provision :ansible do |ansible|
    # point Vagrant at the location of your playbook you want to run
    ansible.playbook = "bin/build.yml"

    # for ansible 1.1 define a hosts file with a vagrant group and the above ip address
    ansible.inventory_path = "bin/local"

  	# ansible 1.5 breaking change, add to use all hosts in provide inventory path above
  	ansible.limit = 'all'

	  ansible.raw_arguments = '--private-key=~/.vagrant.d/insecure_private_key'
  end
end