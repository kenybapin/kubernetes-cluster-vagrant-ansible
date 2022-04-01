# Build a ready-to-use K8s VM cluster


Features: 
- K8s cluster : master + custom number of nodes
- Flannel CNI configured with VM Host-only adapter

&nbsp;

## Requirements
- Ansible 2.9
- Vagrant 2.2.9

&nbsp;

## Setup

1. Git clone to your working directory
```
git clone https://github.com/kenybapin/kubernetes-cluster-vagrant-ansible.git
```
> :warning: Cygwin/WSL1 users, git clone in windows dir (/mnt/c/...)
<br>

2. In Vagrantfile, specify the number of nodes (change N value)
```
# Nodes number
N=4
```
3. Build according to Vagrantfile (This may take a few minutes)
```
vagrant up
```
4. Then, your K8s cluster is up and ready
```
$ vagrant ssh k8smaster

vagrant@k8smaster:~$ kubectl get nodes -owide
NAME        STATUS   ROLES    AGE   VERSION   INTERNAL-IP      EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION       CONTAINER-RUNTIME
k8smaster   Ready    master   25m   v1.18.3   192.168.233.200   <none>        Ubuntu 18.04.4 LTS   4.15.0-101-generic   docker://19.3.11
k8snode-1   Ready    <none>   23m   v1.18.3   192.168.233.101   <none>        Ubuntu 18.04.4 LTS   4.15.0-101-generic   docker://19.3.11
k8snode-2   Ready    <none>   19m   v1.18.3   192.168.233.102   <none>        Ubuntu 18.04.4 LTS   4.15.0-101-generic   docker://19.3.11
k8snode-3   Ready    <none>   16m   v1.18.3   192.168.233.103   <none>        Ubuntu 18.04.4 LTS   4.15.0-101-generic   docker://19.3.11
k8snode-4   Ready    <none>   13m   v1.18.3   192.168.233.104   <none>        Ubuntu 18.04.4 LTS   4.15.0-101-generic   docker://19.3.11
```

&nbsp;

## Deploy Kubernetes Dashboard with remote accees 
> :warning: The following method is not recommended for production use

![alt text](https://github.com/kenybapin/kubernetes-cluster-vagrant-ansible/blob/master/k8s_dashboard.jpg?raw=true)


1. Deploy Kubernetes Dashboard
```
vagrant@k8smaster:~$ kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/master/aio/deploy/recommended.yaml
```
2. Create service account
```
vagrant@k8smaster:~$ kubectl create serviceaccount k8sadmin -n kubernetes-dashboard
vagrant@k8smaster:~$ kubectl create clusterrolebinding k8sadmin --clusterrole=cluster-admin --serviceaccount=kubernetes-dashboard:k8sadmin
```
3. Open a new terminal, and set SSH tunneling for k8smaster
```
@yourPC:~$ ssh -L 8001:127.0.0.1:8001 vagrant@192.168.233.200
```
4. Then on k8smaster, run kubectl proxy service
```
vagrant@k8smaster:~$ kubectl proxy --address 0.0.0.0 --accept-hosts '.*'
```
5. Access to the dashboard
```
# URL (outside VM)
http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login

# get service account token
vagrant@k8smaster:~$ kubectl get secret -n kubernetes-dashboard | grep k8sadmin | cut -d " " -f1 | xargs -n 1 | xargs kubectl get secret  -o 'jsonpath={.data.token}' -n kubernetes-dashboard | base64 --decode
```


