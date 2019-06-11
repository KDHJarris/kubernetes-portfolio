============
INSTALL KUBENETES
============
```
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
exclude=kube*
EOF
```

# Set SELinux in permissive mode (effectively disabling it)

```
setenforce 0
sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config

yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes 
```

==============
INSTALL DOCKER
============== 

```
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

yum install docker-ce docker-ce-cli containerd.io --nobest -y
```

==============
CHANGE HOSTNAMES
============== 
``` 
vi /etc/hostname
---change it to kubemaster, kubenode1, kubenode2
vi /etc/hosts
Remove second line and add in 
```


==========
INITILIZE KUBE
==========
Use below command in conjunction with Flannel, a simple network overlay that meets kubernetes requirements.

```
systemctl start docker
( kubeadm init --pod-network-cidr=10.244.0.0/16 --apiserver-advertise-address=18.191.254.112)



=== WORKING INITIALISATION ====
kubeadm init --pod-network-cidr=10.244.0.0/16 

This command initializes a Kubernetes control-plane node- sets up kubelet, certs, kubeconfig, apiserver, etcd, etc.


===== JOIN KUBE ======


kubeadm join 172.31.8.198:6443 --token n1oe1w.711j5hju6e5c0iud \
    --discovery-token-ca-cert-hash sha256:cad4fb87b172565b30b071ff8ceff02940e0af5b44bbf8122babb3aab2db189


kubeadm join 172.31.8.198:6443 --token ypp0pw.fnq1zpl5r7ru4sof     --discovery-token-ca-cert-hash sha256:cad4fb87b172565b30b071ff8ceff02940e0af5b44bbf8d122babb3aab2db189
sudo kubeadm token create --print-join-command

Actually need to enable traffic from node boxes in security groups

kubectl get nodes

```

========= QUICK DEMO ON WEBSITE AT www.play-with-k8s.com  ========


how to deploy boxes using kube automatically? 

kubeadm init --apiserver-advertise-address $(hostname -i) 

get more info from the pods
kubectl get pods -o wide


=== YAWL Kubernetes ====
pod-definition.yml - has 4 top level properties:
	- apiVersion
	- kind
	- metadata so for expects to have name and labels and labels is a dictionary of key value pairs
	- spec- has property containers- which is a list/array- only can add a single item in the list as it expects one container in the pod. 

once you've filled in the file, you'd use kubectl create -f pod-defintion.yml 

-see the pods created
```
kubectl get pods
kubectl describe pod myapp-pod
```

tip & tricks
- plugins to use like pycharm 
