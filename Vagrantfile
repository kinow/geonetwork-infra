# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV["LC_ALL"] = "en_US.UTF-8"
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # GeoNetwork VM
  config.vm.define "geonetwork" do |geonetwork|
    geonetwork.vm.box = "bento/ubuntu-16.04"
    geonetwork.vm.box_check_update = false

    geonetwork.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--memory", "2048"]
      vb.customize ["modifyvm", :id, "--cpus", "1"]
    end

    geonetwork.vm.network :public_network
    geonetwork.vm.network "private_network", ip: "192.168.100.100"
    geonetwork.vm.network :forwarded_port, guest: 8080, host: 9090, id: "tomcat"
    geonetwork.vm.network "forwarded_port", guest: 22, host: 2222, auto_correct: true, id: "ssh"
    geonetwork.ssh.guest_port = 2222

    # We do not care about security here and want to keep using the default insecure key:
    config.ssh.insert_key = false
  end

  # Postgres + Postgis VM
  config.vm.define "postgres" do |postgres|
    postgres.vm.box = "bento/ubuntu-16.04"
    postgres.vm.box_check_update = false

    postgres.vm.provider "virtualbox" do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1024"]
      vb.customize ["modifyvm", :id, "--cpus", "1"]
    end

    postgres.vm.network :public_network
    postgres.vm.network "private_network", ip: "192.168.100.200"
    postgres.vm.network :forwarded_port, guest: 5432, host: 5432, id: "postgres"
    postgres.vm.network "forwarded_port", guest: 22, host: 2223, auto_correct: true, id: "ssh"
    postgres.ssh.guest_port = 2223

    # We do not care about security here and want to keep using the default insecure key:
    config.ssh.insert_key = false
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.verbose = "vvvv"
    ansible.limit = "all"
    ansible.inventory_path = "hosts"
    ansible.sudo = true
    #ansible.extra_vars = { ansible_ssh_host: '127.0.0.1', ansible_ssh_user: 'vagrant', ansible_ssh_port: 2222 }
  end

end
