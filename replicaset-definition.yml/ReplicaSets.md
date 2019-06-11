=========
Replication controllers and Replicasets
=========

Replicasets is the recommended way to set up replication. 


Spec section defines what's inside the object we are creating 
Can create pod-templates- it can have the similar stuff that's already in metadaba, labels, speccontainers, it should be a child of the Template section. Have nested two definition
files together- one for replicationController nad one for Pod 

```
apiVersion: v1
kind: ReplicationController

metadata:
  name: myapp-pc
  labels:
    name: app
    app: front-end

spec:
  template:

    metadata:
      name: app-prod
      labels:
        app: myapp

    spec:
      containers:
        - name: nginx-container
          image: nginx
replicas: 3

``` 

then you would run the

kubectl create -f rc-definition.yml 
verify: 
kubectl get replicationController

=== ReplicaSet =======

apiVersion: app/v1 
kind: ReplicaSet 


same as other above template except it has selector and it helps the replicaset to identify what parts fall under it- so the replica set can manage parts that was not defined as 
part of ReplcaSet creation.

Selector is a mandatory property of ReplicaSet and the matchLabels has to match the labels 


=============
Labels/selectors
=============

Why use it? 
ReplicaSet is a process that monitor the pods, and there could be hundreds of pods to monitor. 

```
replicaset-definition.yml
selector:
    matchLabels:
       tier: front end

pod-definition.yml
metadata:
 name: myapp-pod
 labels:
   tier: front end
```

in the two files, it helps the replicaSet which part of pods to monitor by looking at the labels. 

=============
Scale up boxes? 
=============

1. Can update the replicaset-defintion file replica key value
 then run kubectl replace -f replicaset-definition.yml

1. kubectl scale --replicas=6 -f replicaset-definition.yml

1. kubectl scale --replicas=6 replicaset myapp-replicaset

                                   ^ type   ^ name



Commands to use: 

```
kubectl create -f replicaset-definition.yml
kubectl get replicaset 
kubectl delete replicaset myapp-replicaset -- deletes all the underlying deployed pods
kubectl replace -f replicaset-definition.yml
```



=======
DEPLOYING THE PODS
=======

kubectl create -f replicaset-definition.yml
kubectl describe replicaset
kubectl get replicaset



====== Replica Controller and why is it important ====== 
Replica Controller helps to run multiple instances in a single node providing high availability


If you have the single pod in a node, it can bring up a new one when the one fails replacing it. 
If you have a single pod serving users, and the demand increases, and resouces run out on the first node, the replica controller can spin up multiple nodes in the cluster.  Helps
us to balance the load across multiple pods across multiple nodes. 

Replica Controller- Older technology and is being replaced by ReplicaSet 

replicacontroller-definition.yml

```
kubectl create -f replicacontroller-definition.yml
```

see the pods created: 

```
kubectl get replicacontroller
kubectl get pods
```

The replicaset needs to have a selector in the replicaSet.yml 

The replicaset/replicas should be monitoring the pods so when one instance fails, replicaset should
automatically bring one up. 

to test it out 

```
kubectl delete pod {nameofpod} 
```

If you try to create a new pod and replicaset will see that it exceeds the total number of replicasets, it will not allow it to start up. Only works if the same labels are used.

If you make updates to the definition file and need to update the state
```
kubectl replace -f replicaset-defintiion.yml
```

can do scale down manually

kubectl scale --replicas=3 -f replicaset-definitiony.ml

kubectl delete replicaset myapp-replicaset



