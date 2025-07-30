# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  # Веб-сервер
  config.vm.define "Dynamicweb" do |web|
    web.vm.box = "ubuntu/jammy64"
    web.vm.hostname = "DynamicWeb"
    web.vm.network "private_network", ip: "192.168.56.10"
    web.vm.network "forwarded_port", guest: 80, host: 8080
    web.vm.network "forwarded_port", guest: 443, host: 8443
    web.vm.network "forwarded_port", guest: 9100, host: 9100

    web.vm.provider "virtualbox" do |vbx|
      vbx.memory = 2048
      vbx.cpus = 2
      vbx.customize ["modifyvm", :id, "--audio", "none"]
    end
  end

  # Сервер мониторинга
  config.vm.define "Grafana" do |mon|
    mon.vm.box = "ubuntu/jammy64"
    mon.vm.hostname = "Monitoring"
    mon.vm.network "private_network", ip: "192.168.56.20"
    mon.vm.network "forwarded_port", guest: 9090, host: 9090   # Prometheus
    mon.vm.network "forwarded_port", guest: 3000, host: 3000   # Grafana
    mon.vm.network "forwarded_port", guest: 514, host: 5144

    mon.vm.provider "virtualbox" do |vbx|
      vbx.memory = 2048
      vbx.cpus = 2
      vbx.customize ["modifyvm", :id, "--audio", "none"]
    end
  end

  # Provisioning через Ansible 
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "prov.yml"
    ansible.verbose = "v"
  end

end
