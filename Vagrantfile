# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

NUM_CONTROLLERS = ENV['NUM_CONTROLLERS'] || 2

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.ssh.forward_agent = true

  (0..NUM_CONTROLLERS).each do |i|
    config.vm.define "zone#{i}" do |controller_config|
      controller_config.apt_proxy.http  = "http://192.168.122.1:3142"
      controller_config.apt_proxy.https = "DIRECT"
      controller_config.vm.box = "ubuntu/trusty64"
      controller_config.ssh.insert_key = false
      controller_config.vm.hostname = "zone#{i}"
      controller_config.vm.network :private_network, ip: "172.100.0.10#{i}",
        auto_config: false
      controller_config.vm.network :private_network, ip: "172.200.0.10#{i}",
        auto_config: false
      controller_config.vm.provider "virtualbox" do |v|
        v.memory = 512
        v.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
        v.customize ["modifyvm", :id, "--nicpromisc3", "allow-all"]
      end
      config.vm.provision "ansible" do |ansible|
        ansible.playbook = "site.yml"
      end
    end
  end

end
