# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.define "sentry" do |sentry|
    sentry.vm.box = "centos-6-4"
    # sentry.vm.box_url = "http://files.vagrantup.com/precise32.box"
    sentry.vm.network :private_network, ip: "192.168.33.10"

    sentry.vm.provision "ansible" do |ansible| 
      ansible.playbook = "playbooks/sentry.yml"
      ansible.verbose = 'vvv'
    end 
  end

end


