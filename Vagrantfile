# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.define "k8smaster" do |k8smaster|
    k8smaster.vm.box = "bento/ubuntu-18.04"
    k8smaster.vm.hostname = 'k8smaster'
    k8smaster.vm.provision "docker"
    k8smaster.vm.box_url = "bento/ubuntu-18.04"
    k8smaster.vm.network :private_network, ip: "192.168.56.200"
    k8smaster.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 2048]
      v.customize ["modifyvm", :id, "--name", "k8smaster"]
      v.customize ["modifyvm", :id, "--cpus", "2"]
    end
		k8smaster.vm.provision "ansible_local" do |ansible|
			ansible.playbook = "master-playbook.yml"
			ansible.extra_vars = {
				node_ip: "192.168.56.200",
			}
		end
  end

# Nodes number
N=4

  (1..N).each do |i|
		config.vm.define "k8snode-#{i}" do |k8snode|
	  	k8snode.vm.box = "bento/ubuntu-18.04"
	  	k8snode.vm.hostname = "k8snode-#{i}"
	  	k8snode.vm.provision "docker"
	  	k8snode.vm.box_url = "bento/ubuntu-18.04"
	  	k8snode.vm.network :private_network, ip: "192.168.56.#{i + 100}"
	  	k8snode.vm.provider :virtualbox do |v|
	  	  v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
	  	  v.customize ["modifyvm", :id, "--memory", 2048]
	  	  v.customize ["modifyvm", :id, "--name", "k8snode-#{i}"]
	  	  v.customize ["modifyvm", :id, "--cpus", "2"]
	  	end
	  	k8snode.vm.provision "ansible_local" do |ansible|
	  	  ansible.playbook = "node-playbook.yml"
	  	  ansible.extra_vars = {
	  		  node_ip: "192.168.56.#{i + 100}",
	  		}
	  	end
		end
	end
end
