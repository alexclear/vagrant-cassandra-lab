# -*- mode: ruby -*-
# vi: set ft=ruby :

$CASSANDRA1_IP = "172.16.147.2"
$CASSANDRA2_IP = "172.16.147.3"
$CASSANDRA3_IP = "172.16.147.4"
$CASSANDRA1_NUM_CPUS = 2
$CASSANDRA2_NUM_CPUS = 2
$CASSANDRA3_NUM_CPUS = 2
$CASSANDRA1_MEM_MBS = 2048
$CASSANDRA2_MEM_MBS = 2048
$CASSANDRA3_MEM_MBS = 2048

Vagrant.configure("2") do |config|
  config.vm.define "cassandra1" do |cassandra1|
    cassandra1.vm.box = "centos/7"
    cassandra1.vm.hostname = "cassandra1"
    cassandra1.vm.network "private_network", ip: $CASSANDRA1_IP

    cassandra1.vm.provider :virtualbox do |v, override|
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", $CASSANDRA1_NUM_CPUS]
      v.customize ["modifyvm", :id, "--memory", $CASSANDRA1_MEM_MBS]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    cassandra1.vm.provider :libvirt do |v, override|
      v.cpu_mode = 'custom'
      v.cpu_model = 'kvm64'
      v.volume_cache = 'writeback'
      v.disk_bus = 'virtio'
      v.cpus = $CASSANDRA1_NUM_CPUS
      v.memory = $CASSANDRA1_MEM_MBS
    end

    cassandra1.vm.provision "shell", inline: "yum -y install python git"
    cassandra1.vm.provision "shell", inline: "sysctl net.ipv6.conf.all.disable_ipv6=1"
    cassandra1.vm.provision "shell", inline: "sysctl net.ipv6.conf.default.disable_ipv6=1"
  end

  config.vm.define "cassandra2" do |cassandra2|
    cassandra2.vm.box = "centos/7"
    cassandra2.vm.hostname = "cassandra2"
    cassandra2.vm.network "private_network", ip: $CASSANDRA2_IP

    cassandra2.vm.provider :virtualbox do |v, override|
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", $CASSANDRA2_NUM_CPUS]
      v.customize ["modifyvm", :id, "--memory", $CASSANDRA2_MEM_MBS]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    cassandra2.vm.provider :libvirt do |v, override|
      v.cpu_mode = 'custom'
      v.cpu_model = 'kvm64'
      v.volume_cache = 'writeback'
      v.disk_bus = 'virtio'
      v.cpus = $CASSANDRA2_NUM_CPUS
      v.memory = $CASSANDRA2_MEM_MBS
    end

    cassandra2.vm.provision "shell", inline: "yum -y install python git"
    cassandra2.vm.provision "shell", inline: "sysctl net.ipv6.conf.all.disable_ipv6=1"
    cassandra2.vm.provision "shell", inline: "sysctl net.ipv6.conf.default.disable_ipv6=1"
  end

  config.vm.define "cassandra3" do |cassandra3|
    cassandra3.vm.box = "centos/7"
    cassandra3.vm.hostname = "cassandra3"
    cassandra3.vm.network "private_network", ip: $CASSANDRA3_IP

    cassandra3.vm.provider :virtualbox do |v, override|
      v.gui = false
      v.customize ["modifyvm", :id, "--cpus", $CASSANDRA3_NUM_CPUS]
      v.customize ["modifyvm", :id, "--memory", $CASSANDRA3_MEM_MBS]
      v.customize ["modifyvm", :id, "--cableconnected1", "on"]
      v.customize ["modifyvm", :id, "--cableconnected2", "on"]
    end

    cassandra3.vm.provider :libvirt do |v, override|
      config.vm.synced_folder ".", "/vagrant", type: "nfs"
      v.cpu_mode = 'custom'
      v.cpu_model = 'kvm64'
      v.volume_cache = 'writeback'
      v.disk_bus = 'virtio'
      v.cpus = $CASSANDRA3_NUM_CPUS
      v.memory = $CASSANDRA3_MEM_MBS
    end

    cassandra3.vm.provision "shell", inline: "yum -y install python git"
    cassandra3.vm.provision "shell", inline: "sysctl net.ipv6.conf.all.disable_ipv6=1"
    cassandra3.vm.provision "shell", inline: "sysctl net.ipv6.conf.default.disable_ipv6=1"
    cassandra3.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "ansible/site.yml"
      ansible.config_file = "ansible/ansible.cfg"
      ansible.inventory_path = "ansible/inventory"
      ansible.become = true
      ansible.galaxy_role_file = "ansible/roles/requirements.yml"
      ansible.galaxy_roles_path = "/etc/ansible/roles"
      ansible.galaxy_command = "sudo ansible-galaxy install --role-file=%{role_file} --roles-path=%{roles_path} --force"
      ansible.limit = "all"
    end
  end
end
