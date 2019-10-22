# -*- mode: ruby -*-
# vi: set ft=ruby :
# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
    # Using Centos for the base image
    config.vm.box = "centos/7"

    # config.ssh.username = "vagrant"
    # config.ssh.password = "vagrant"
    # config.ssh.insert_key = true

    config.vm.define "AEM-UP" do |aemup|
    end

    config.vm.provider "virtualbox" do |vb|
        # AEM recomments at least 1920 mb to work properly
        vb.memory = "5000"
        vb.cpus = 2
        vb.customize ["modifyvm", :id, "--audio", "none"]
    end

    # Disable default rsync folder and setup new one
    config.vm.synced_folder ".", "/vagrant", disabled: true
    config.vm.synced_folder "./user-provided", "/home/vagrant/user-provided", disabled: false, type: "rsync", create: true

    # attempting to sync logs.. not working yet.. :(
    # config.vm.synced_folder "./instances", "/home/vagrant", type: "nfs", disabled: false

    # Creates host-only static ip address on the machine's private network
    config.vm.network "private_network", ip: "192.168.99.44"

    # Ports Forwarding
    config.vm.network "forwarded_port", guest: 4502, host: 4502
    config.vm.network "forwarded_port", guest: 4503, host: 4503
    config.vm.network "forwarded_port", guest: 80, host: 4604

    # Run ansible playbook
    config.vm.provision "ansible" do |ansible|
        ansible.verbose = "v"
        ansible.playbook = "playbook.yml"
        ansible.compatibility_mode = "2.0"
        ansible.become = true # run everything as root, dev box remember :)
        # see: https://gist.github.com/phantomwhale/9657134#gistcomment-1195013
        # basically if you want to pass extra params to ansible, for example, run aem_dispatcher taged tasks:
        # run this `ANSIBLE_ARGS='--tags "aem_dispatcher"' vagrant provision`
        ansible.raw_arguments = Shellwords.shellsplit(ENV['ANSIBLE_ARGS']) if ENV['ANSIBLE_ARGS']
    end
end
