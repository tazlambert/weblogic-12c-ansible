# -*- mode: ruby -*-
# vi: set ft=ruby :

#setting Vagrant API Version
VAGRANTFILE_API_VERSION = "2"
#setting number of vm
N = 2
#adding oracle linux box
BOX_URL = "https://oracle.github.io/vagrant-projects/boxes"
BOX_NAME = "oraclelinux/7"

Vagrant.configure(VAGRANTFILE_API_VERSION) do|config|

(1..N).each do |machine_id|
  config.vm.define "app#{machine_id}" do |machine|
    machine.vm.hostname = "app#{machine_id}"
    machine.vm.box= "oraclelinux/7"
    machine.vm.box_url = "#{BOX_URL}/#{BOX_NAME}.json"
    machine.vm.network "private_network", ip: "192.168.56.#{20+machine_id}"

  config.vm.provision "file", source: "/home/opc/.ssh/id_rsa", destination: "/home/vagrant/.ssh/id_rsa"
  public_key = File.read("/home/opc/.ssh/id_rsa.pub")
  config.vm.provision :shell, :inline =>"
     echo 'Copying ansible-vm public SSH Keys to the VM'
     mkdir -p /home/vagrant/.ssh
     chmod 700 /home/vagrant/.ssh
     echo '#{public_key}' >> /home/vagrant/.ssh/authorized_keys
     chmod -R 600 /home/vagrant/.ssh/authorized_keys
     echo 'Host 192.168.*.*' >> /home/vagrant/.ssh/config
     echo 'StrictHostKeyChecking no' >> /home/vagrant/.ssh/config
     echo 'UserKnownHostsFile /dev/null' >> /home/vagrant/.ssh/config
     chmod -R 600 /home/vagrant/.ssh/config
     ", privileged: false

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 4096
    vb.cpus = 2
  end

    # Only execute once the Ansible provisioner,
    # when all the machines are up and ready.
    if machine_id == N
      machine.vm.provision :ansible do |ansible|
        ansible.playbook = "playbook.yml"
        ansible.inventory_path = "./hosts"
        ansible.limit = 'all'
      end
    end
  end
end
end
