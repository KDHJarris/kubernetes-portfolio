======== Deployments =========

Not much difference between replicaSet, but the kind is Deployment and creates a new kubernetes object called
deployments.

To see all kubernetes objects: 
```
kubectl get all 
```

==== Versioning and Rollouts ======

In production, you will most likely be using deployment-definition files directly.


```
kubectl rollout status deployment/myapp-deployment
```

2 types of deployment strategies.

1. Recreate: In order to upgrade to a newer version, you have to destory all of the instances and then create new version of app instnaces. 
Destroy 5, then deploy 5 new ones.  == DOWNTIME . Not the default method.
2. Rolling update where one is taken down one by one and is the default strategy if you dont define it. 

If you make changes to definition files, can run the following command
```
kubectl apply -f deployment-definition.yml
```

another way to do same thing

kubectl set image deployment/myapp-deployment \ nginx=nginx:1.9.1


 === Upgrades ===
 
how does it do it under the hood: 

When doing a deployment using rolling update strategy, replicaset-1 and the updates creates a replica-set-2 and pods are then terminated in the old pods. 

If you need to do a rollback, can do the following
```
kubectl rollout undo deployment/myapp-deployment 
```

**kubectl run** is another way of specifying a deployment without a definition file and specifying the image name.

Create: kubectl create -f deployment-definition.yml 

**Get:** kubectl get deployments

**Update:** kubectl apply -f deployment-definition.yml
        kubectl set image deployment/myapp-deployment \ nginx=nginx:1.9.1
        
**Status:** kubectl rollout status deployment/myapp-deployment
        kubectl rollout history deployment/myapp-deployment
        
**Rollback:**  kubectl rollout undo deployment/myapp-deployment


**Description:** kubectl describe deployment 




3.17.75.117
18.191.144.189


C:/Users/KAEDIHarris/Documents/AWS_Key_Pair.pem