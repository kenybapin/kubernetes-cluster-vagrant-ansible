# Kubernetes quick start
**Build a ready-to-use Kubernetes cluster on Virtualbox with Ansible & Vagrant**

**Features**  
- K8s cluster : 2 VMs (master + worker)
- Flannel CNI configured with Host-only adapter (VirtualBox) 
___
## 1. Pre-requisites
- Linux or Windows with WSL1 
- VirtualBox 6.0 (6.0 is compatible with vagrant)
<br><br>
## 2. Setup
### Ansible 
**1. Installation**
```bash
sudo apt-get update && sudo apt-get upgrade && sudo apt-get autoremove
sudo apt-get install ansible
sudo apt-get install -y python-pip libssl-dev
```
**2. Check**
```bash
which ansible
ansible --version
```

**3. Create a new test playbook:** ansible-test.yml
```yaml
---
- hosts: localhost
  tasks:
    - debug: msg="Ansible is working!"
```

4. **Run the playbook**
```bash
ansible-playbook ansible-test.yml --connection=local
```
<sup>**Ansible might warn about no inventory file being present, but since you're using --connection=local, the localhost host should automatically work.**<sup><br><br>

### Vagrant

**1. Installation**

***Linux***
- Install vagrant debian package https://www.vagrantup.com/downloads
```bash
wget https://releases.hashicorp.com/vagrant/2.2.9/vagrant_2.2.9_x86_64.deb
sudo dpkg -i vagrant_2.2.9_x86_64.deb
sudo apt-get -y install libvirt-dev
```
***Windows***
- Install vagrant for windows https://www.vagrantup.com/downloads
- For WSL1 users, export or add these commands to the bottom of your shell (~/.bashrc or ~/.zshrc)
```bash
export PATH="$PATH:/mnt/c/Program Files/Oracle/VirtualBox"
export VAGRANT_WSL_ENABLE_WINDOWS_ACCESS="1"
```
- Then, reboot your machine.

:warning: Unfortunately, WSL2 is not compatible with Vagrant. You'll have to disable WSL2 + Hyper-V, then revert your WSL dist. to WSL1


**2. Check** : In your HOME dir, create a new VirtualBox VM
```bash
vagrant --version
vagrant init alpine/alpine64
vagrant up
```
&nbsp;
## 3. Build your kubernetes cluster
**1. Copy these manifests to your working Dir**  
```
git clone https://github.com/kenybapin/kubernetes-cluster-vagrant-ansible.git
cp kubernetes-cluster-vagrant-ansible/* /c/Users/keny/Documents/vagrant
```
**2. Build according to Vagrantfile (This may take a few minutes)**
```
cd /c/Users/keny/Documents/vagrant
vagrant up
```
**3. Then, your K8s cluster is up and ready**
```
vagrant ssh k8smaster

vagrant@k8smaster:~$ kubectl get nodes
NAME        STATUS   ROLES    AGE    VERSION
k8smaster   Ready    master   112m   v1.18.2
k8snode     Ready    <none>   109m   v1.18.2

```
