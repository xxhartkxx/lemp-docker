# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box     = "my/centos-7.2"
  config.vm.box_url = "http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_centos-7.2_chef-provisionerless.box"

  config.ssh.insert_key = false

#  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.network "private_network", ip: "192.168.33.11"

  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--memory", "4096", "--cpus", "2", "--ioapic", "on"]
  end

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook       = "ansible/site.yml"
    ansible.install        = true
    ansible.inventory_path = "ansible/hosts"
    ansible.limit          = 'all'
  end
end
