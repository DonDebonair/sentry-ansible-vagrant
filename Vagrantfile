# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.define "sentry" do |sentry|

    ENV['LC_ALL']="en_US.UTF-8"
    ENV['LC_ADDRESS']="en_US.UTF-8"
    ENV['LC_MEASUREMENT']="en_US.UTF-8"
    ENV['LC_NUMERIC']="en_US.UTF-8"
    ENV['LC_ALL']="en_US.UTF-8"
    ENV['LC_MONETARY']="en_US.UTF-8"
    ENV['LC_PAPER']="en_US.UTF-8"
    ENV['LC_IDENTIFICATION']="en_US.UTF-8"
    ENV['LC_NAME']="en_US.UTF-8"
    ENV['LC_TELEPHONE']="en_US.UTF-8"

    sentry.vm.provider :virtualbox do |provider, override|
      override.vm.box = "ubuntu/trusty64"
      override.vm.network :private_network, ip: "192.168.33.10"
    end

    sentry.vm.provider :digital_ocean do |provider, override|
      override.vm.box = "digital_ocean"
      override.ssh.private_key_path = 'redacted'
      provider.client_id = "getsentry"
      provider.token = "redacted"
      provider.image = "ubuntu-12-04-x64"
      provider.region = "nyc2"
      provider.size = "1gb"
    end

    sentry.vm.provision "ansible" do |ansible| 
      ansible.playbook = "sentry.yml"
      ansible.verbose = 'vvv'
    end 

  end
end
