# -*- mode: ruby -*-
# vi: set ft=ruby :
box_name = "15-heroku"
static_ip = "192.168.111.8"

Vagrant.configure("2") do |config|

    #config.vm.host_name = "#{box_name}"
    config.vm.box = "#{box_name}"

    config.vm.network :forwarded_port, guest: 80, host: 3000
    config.vm.network :forwarded_port, guest: 1080, host: 3001

    #might we need this for nfs?
    config.vm.network :private_network, ip: "#{static_ip}"

    #copy user settings folder at some point

    # share a local code folder for editing in a native env
    config.vm.synced_folder "/code/vagrant/#{box_name}/code", "/code", :nfs => true

    # share your rvm path so we can search gem file code with vim
    config.vm.synced_folder "/code/vagrant/#{box_name}/vm-rvm-path", "/home/vagrant/.rvm", :nfs => true

    # The environment key to set
    # this lets the vm know where the files are for the better_errors gem.
    config.hostpath.env_key = "http_VAGRANT_HOST_PATH"

    config.hostpath.path_file = "/tmp/.vagrant-host-path"

    # Profile script path
    config.hostpath.profile_path = "/etc/profile.d/vagrant-host-path.sh"
    
    config.vm.provision :ansible do |ansible|
      ansible.playbook = "provisioning/playbook.yml"
      ansible.inventory_file = "provisioning/ansible_hosts"
      ansible.verbose = "vvv" 
      ansible.extra_vars = { vagrant_static_ip: "#{static_ip}" }

      # actually changed ansible/provisioner.rb in the vagrant 
      # code to make this debug output work for the command ansible-playbook
      # may break for other versions. 
      ansible.options = "-vvv"
    end
end 
