# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|

  # Ensure we use our vagrant private key
  config.ssh.insert_key = false
  config.ssh.private_key_path = '~/.vagrant.d/insecure_private_key'

  config.vm.define 'cimon-ansible' do |machine|
    machine.vm.box = "debian/stretch64"
    #machine.vm.box = "bento/ubuntu-16.04"
    #machine.vm.box = "centos/7"

    machine.vm.network :private_network, ip: '192.168.101.11'
    machine.vm.hostname = 'cimon-ansible.local'

    machine.vm.provision 'ansible' do |ansible|
      ansible.playbook = 'tests/playbook.yml'
      ansible.become = true
      ansible.inventory_path = 'vagrant-inventory'
      ansible.host_key_checking = false
    end

  end

end
