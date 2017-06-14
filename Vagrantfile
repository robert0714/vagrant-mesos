# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  if (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
    config.vm.synced_folder ".", "/vagrant", mount_options: ["dmode=700,fmode=600"]
  else
    config.vm.synced_folder ".", "/vagrant"
  end
  config.vm.define "cd" do |d|
    d.vm.box = "ubuntu/trusty64"
    d.vm.hostname = "cd"
    d.vm.network "private_network", ip: "10.100.98.200"
    d.vm.provision :shell, path: "scripts/bootstrap_ansible.sh"
    #d.vm.provision :shell, inline: "PYTHONUNBUFFERED=1 ansible-playbook /vagrant/ansible/mesos.yml -i /vagrant/ansible/hosts/prod"
    d.vm.provider "virtualbox" do |v|
      v.memory = 2048
    end
  end  
  (1..4).each do |i|
    config.vm.define "mesos-#{i}" do |d|
      d.vm.box = "bento/centos-7.3"
 #     d.vm.box = "ubuntu/trusty64"
      d.vm.hostname = "mesos-#{i}"
      d.vm.network "private_network", ip: "10.100.97.20#{i}"     
      d.vm.provider "virtualbox" do |v|
        v.memory = 2536
      end
    end
  end
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end
  if Vagrant.has_plugin?("vagrant-vbguest")
    config.vbguest.auto_update = false
    config.vbguest.no_install = true
    config.vbguest.no_remote = true
  end
end
