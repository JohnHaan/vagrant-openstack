# -*- mode: ruby -*-
# vi: set ft=ruby :

sdc = 'C:\Users\sjhan\git\openstack_vagrant\openstack-src-xenial64\sdc.vdi'
sdd = 'C:\Users\sjhan\git\openstack_vagrant\openstack-src-xenial64\sdd.vdi'
sde = 'C:\Users\sjhan\git\openstack_vagrant\openstack-src-xenial64\sde.vdi'

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define "src-ceph01" do |ceph|
    ceph.vm.box = "ubuntu/xenial64"
    ceph.vm.hostname = "src-ceph01"
    ceph.vm.network "private_network", ip: "192.168.99.201"   ### MGMT,TUNNEL IP
    ceph.vm.network "private_network", ip: "192.168.56.201"   ### EXTERNAL IP
#    ceph.vm.network "private_network", ip: "10.20.0.201"   ### MGMT_IP
    ceph.ssh.forward_agent = false
    ceph.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
      unless File.exist?(sdc)
        vb.customize ['createhd', '--filename', sdc, '--size', 50 * 1024]
      end
      vb.customize ['storageattach', :id, '--storagectl', 'SCSI Controller', '--port', 3, '--device', 0, '--type', 'hdd', '--medium', sdc]
      unless File.exist?(sdd)
        vb.customize ['createhd', '--filename', sdd, '--size', 50 * 1024]
      end
      vb.customize ['storageattach', :id, '--storagectl', 'SCSI Controller', '--port', 4, '--device', 0, '--type', 'hdd', '--medium', sdd]
      unless File.exist?(sde)
        vb.customize ['createhd', '--filename', sde, '--size', 50 * 1024]
      end
      vb.customize ['storageattach', :id, '--storagectl', 'SCSI Controller', '--port', 5, '--device', 0, '--type', 'hdd', '--medium', sde]
    end 
    ceph.vm.provision :shell, :path => "common_script"
  end
 
  config.vm.define "src-controller" do |v|
    v.vm.box = "ubuntu/xenial64"
    v.vm.hostname = "src-controller"
    v.vm.network "private_network", ip: "192.168.99.200"   ### MGMT,TUNNEL IP
    v.vm.network "private_network", ip: "192.168.56.200"   ### EXTERNAL IP
#    v.vm.network "private_network", ip: "10.20.0.200"   ### EXTERNEL_IP
    v.ssh.forward_agent = false
    v.vm.provider "virtualbox" do |vb|
      vb.memory = 4096
      vb.cpus = 2 
    end
    v.vm.provision :shell, :path => "common_script"
    v.vm.provision :shell, :path => "controller_scripts"
  end
  
  config.vm.define "src-compute01" do |y|
    y.vm.box = "ubuntu/xenial64"
    y.vm.hostname = "src-compute01"
    y.vm.network "private_network", ip: "192.168.99.202"   ### MGMT,TUNNEL IP
    y.vm.network "private_network", ip: "192.168.56.202"   ### EXTERNAL IP
#    y.vm.network "private_network", ip: "10.20.0.202"   ### EXTERNEL_IP
    y.ssh.forward_agent = false
    y.vm.provider "virtualbox" do |yb|
      yb.memory = 2048
      yb.cpus = 2 
    end
    y.vm.provision :shell, :path => "common_script"
    y.vm.provision :shell, :path => "compute_scripts"
  end  
 end