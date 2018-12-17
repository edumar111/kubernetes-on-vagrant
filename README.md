# INSTALL CLUSTER KUBERNETES

we are going to install __kubernetes__ in a cluster of virtual machines, which were provided with vagrant.

The cluster on  two node and  manager.

### requirements
 - vagrant

### configuring the vitual machines 

start the virtual machines

	$ vagrant up

Enter in each   virtual machine:

	$ vagrant ssh manager
	$ vagrant ssh node1
	$ vagrant ssh node2


disabled swap in each virtual machine of the cluster


	$ sudo nano /etc/fstab

comment file  

`#/dev/mapper/VolGroup00-LogVol01 swap`

config selinux in each virtual machine of the cluster

	$ sudo nano /etc/selinux/config

`SELINUX=disabled`
	
	$ sudo yum update

Reboot cluster
	
	$ vagrant reload


 verified in the virtual machines that selinux is disabled
	
	$ sestatus



## install Kubernetes
in each virtual machine of the cluster


	$ sudo yum install -y kubelet kubeadm kubectl
	$ sudo su

	$ systemctl enable kubelet && sudo systemctl start kubelet

Reboot cluster
	
	$ vagrant reload


before verified ips in each virtual machine 
	
	$ ip a

config hosts in each virtual machine of the cluster,

	$ sudo nano /etc/hosts

add in file

```
192.168.1.7 node1
192.168.1.5 node2
192.168.1.8 manager
```

### manager 
init cluster kubernetes
note: manager requires 2 cpus

	$ vagrant ssh manager
	$ sudo kubeadm init


join node1 and node2 execute 
token join on master
 
 kubeadm join 192.168.1.8:6443 --token hxsc56.qc8vn2zbepwxjw59 --discovery-token-ca-cert-hash sha256:2075c05627749013bbd0fcfb044173fcc9f0df451f789cb844c5808d41585a46

To start using your cluster, you need to run the following as a regular user:

	$ mkdir -p $HOME/.kube
  	$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  	$ sudo chown $(id -u):$(id -g) $HOME/.kube/config

but if you are root(for this time just run)

	$ export KUBECONFIG=/etc/kubernetes/admin.conf

virtualize the network and create roles

	$ kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"

verifiy 

	$  kubectl get pods --all-namespaces



 	
### node1
	$ sudo kubeadm join 192.168.1.8:6443 --token hxsc56.qc8vn2zbepwxjw59 --discovery-token-ca-cert-hash sha256:2075c05627749013bbd0fcfb044173fcc9f0df451f789cb844c5808d41585a46

### node2
	$ sudo   kubeadm join 192.168.1.8:6443 --token hxsc56.qc8vn2zbepwxjw59 --discovery-token-ca-cert-hash sha256:2075c05627749013bbd0fcfb044173fcc9f0df451f789cb844c5808d41585a46


##validate join nodes in manager

	$ kubectl get nodes

##Deployment n manager

	$ sudo nano nginx-deployment.yaml

copy

```

apiVersion: apps/v1 # for versions before 1.9.0 use apps/v1beta2
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  replicas: 2 # tells deployment to run 2 pods matching the template
  template: # create pods using pod definition in this template
    metadata:
      # unlike pod-nginx.yaml, the name is not included in the meta data as a unique name is
      # generated from the deployment name
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80	

```

deployment

	$ kubectl apply -f nginx-deployment.yaml 

verify deployment

	$ kubectl describe deployment nginx-deployment.yaml

view pods 
	
	kubectl get pods -l app=nginx

describe pods

	kubectl describe pod nginx-deployment-67594d6bf6-bgm8g


Eliminando el deployment
	
	$ kubectl delete -f nginx-deployment.yaml 