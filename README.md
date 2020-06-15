# Kubernetes quick start
**Build a ready-to-use Kubernetes cluster on Virtualbox with Ansible & Vagrant**

**Features**  
- K8s cluster : master + custom number of nodes
- Flannel CNI configured with Host-only adapter (VirtualBox) 
___
## Pre-requisites
- VirtualBox
- Ansible
- Vagrant
<br><br>

## Pre-requisites notes 
<details>
  <summary>Click to expand!</summary>

<br>
Ansible

```
1. Installation

$ sudo apt-get update && sudo apt-get upgrade && sudo apt-get autoremove
$ sudo apt-get install ansible
$ sudo apt-get install -y python-pip libssl-dev

2. Check installation

$ which ansible
$ ansible --version

  2.1 Create a new test playbook: ansible-test.yml

---
- hosts: localhost
  tasks:
    - debug: msg="Ansible is working!"
    

  2.2 Run the playbook

$ ansible-playbook ansible-test.yml --connection=local

# Note: Ansible might warn about no inventory file being present, but since you're using --connection=local, the localhost host should automatically work.
```

<br>
Vagrant

```
1. Installation

## Linux

# Install vagrant debian package https://www.vagrantup.com/downloads

$ wget https://releases.hashicorp.com/vagrant/2.2.9/vagrant_2.2.9_x86_64.deb
$ sudo dpkg -i vagrant_2.2.9_x86_64.deb
$ sudo apt-get -y install libvirt-dev

## Windows

# Install vagrant for windows https://www.vagrantup.com/downloads
# For WSL1 users, export or add these commands to your shell (~/.bashrc or ~/.zshrc)

$ export PATH="$PATH:/mnt/c/Program Files/Oracle/VirtualBox"
$ export VAGRANT_WSL_ENABLE_WINDOWS_ACCESS="1"

# Then, reboot your machine.

# Note: Unfortunately, WSL2 is not yet compatible with Vagrant. You'll have to disable WSL2 + Hyper-V, then revert your WSL dist. to WSL1

2. Check installation

# Create a new VirtualBox VM

$ vagrant --version
$ vagrant init alpine/alpine64
$ vagrant up
```

</details>

&nbsp;

## Installation
**1. Copy all files to your working directory**  
```
git clone https://github.com/kenybapin/kubernetes-cluster-vagrant-ansible.git
cp kubernetes-cluster-vagrant-ansible/* /c/Users/keny/vagrant
```
**2. In Vagrantfile, specify the number of nodes (change N value)**
```
# Nodes number
N=4
```
**3. Build according to Vagrantfile (This may take a few minutes)**
```
cd /c/Users/keny/vagrant
vagrant up
```
**4. Then, your K8s cluster is up and ready**
```
$ vagrant ssh k8smaster

vagrant@k8smaster:~$ kubectl get nodes -owide
NAME        STATUS   ROLES    AGE   VERSION   INTERNAL-IP      EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION       CONTAINER-RUNTIME
k8smaster   Ready    master   25m   v1.18.3   192.168.56.200   <none>        Ubuntu 18.04.4 LTS   4.15.0-101-generic   docker://19.3.11
k8snode-1   Ready    <none>   23m   v1.18.3   192.168.56.101   <none>        Ubuntu 18.04.4 LTS   4.15.0-101-generic   docker://19.3.11
k8snode-2   Ready    <none>   19m   v1.18.3   192.168.56.102   <none>        Ubuntu 18.04.4 LTS   4.15.0-101-generic   docker://19.3.11
k8snode-3   Ready    <none>   16m   v1.18.3   192.168.56.103   <none>        Ubuntu 18.04.4 LTS   4.15.0-101-generic   docker://19.3.11
k8snode-4   Ready    <none>   13m   v1.18.3   192.168.56.104   <none>        Ubuntu 18.04.4 LTS   4.15.0-101-generic   docker://19.3.11
```
