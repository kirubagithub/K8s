# Deployment
```
k create -f deployment.yaml
k describe deployment myapp-deployment
k get deploy
k get pods
k get all
k delete deployment myapp-deployment
```

# Rollout and Rollback

``` 
k create -f deployment.yaml 
k rollout status deployment.apps/myapp-deployment 
```

# History


#### No history
k rollout history deployment.apps/myapp-deployment 
#### To make history (shown in Annotations in describe deployment)
```
k create -f deployment.yaml --record

k rollout history deployment.apps/myapp-deployment

k edit deployment myapp-deployment --record

k rollout status deployment.apps/myapp-deployment
```

``` 
 ~/k8s/code/K8s/deployments/ [main*] k rollout status deployment.apps/myapp-deployment
Waiting for deployment "myapp-deployment" rollout to finish: 1 out of 3 new replicas have been updated...
Waiting for deployment "myapp-deployment" rollout to finish: 1 out of 3 new replicas have been updated...
Waiting for deployment "myapp-deployment" rollout to finish: 1 out of 3 new replicas have been updated...
Waiting for deployment "myapp-deployment" rollout to finish: 2 out of 3 new replicas have been updated...
Waiting for deployment "myapp-deployment" rollout to finish: 2 out of 3 new replicas have been updated...
Waiting for deployment "myapp-deployment" rollout to finish: 1 old replicas are pending termination...
Waiting for deployment "myapp-deployment" rollout to finish: 1 old replicas are pending termination...
deployment "myapp-deployment" successfully rolled out
 ~/k8s/code/K8s/deployments/ [main*] k get pods
NAME                                READY   STATUS    RESTARTS   AGE
myapp-deployment-64d6bbc45b-drbjm   1/1     Running   0          12s
myapp-deployment-64d6bbc45b-dzfps   1/1     Running   0          20s
myapp-deployment-64d6bbc45b-wtrr2   1/1     Running   0          10s
 ~/k8s/code/K8s/deployments/ [main*]
```


k describe deploy myapp-deployment
```
 ~/k8s/code/K8s/deployments/ [main*] k describe deploy myapp-deployment
Name:                   myapp-deployment
Namespace:              default
CreationTimestamp:      Mon, 27 May 2024 22:41:42 +0100
Labels:                 app=nginx
                        tier=frontend
Annotations:            deployment.kubernetes.io/revision: 2
                        kubernetes.io/change-cause: kubectl edit deployment myapp-deployment --record=true
Selector:               env=myapp
Replicas:               3 desired | 3 updated | 3 total | 3 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  env=myapp
  Containers:
   nginx:
    Image:        nginx:1.21
    Port:         <none>
    Host Port:    <none>
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Available      True    MinimumReplicasAvailable
  Progressing    True    NewReplicaSetAvailable
OldReplicaSets:  myapp-deployment-7bf54bc64d (0/0 replicas created)
NewReplicaSet:   myapp-deployment-64d6bbc45b (3/3 replicas created)
Events:
  Type    Reason             Age   From                   Message
  ----    ------             ----  ----                   -------
  Normal  ScalingReplicaSet  5m    deployment-controller  Scaled up replica set myapp-deployment-7bf54bc64d to 3
  Normal  ScalingReplicaSet  96s   deployment-controller  Scaled up replica set myapp-deployment-64d6bbc45b to 1
  Normal  ScalingReplicaSet  88s   deployment-controller  Scaled down replica set myapp-deployment-7bf54bc64d to 2 from 3
  Normal  ScalingReplicaSet  88s   deployment-controller  Scaled up replica set myapp-deployment-64d6bbc45b to 2 from 1
  Normal  ScalingReplicaSet  86s   deployment-controller  Scaled down replica set myapp-deployment-7bf54bc64d to 1 from 2
  Normal  ScalingReplicaSet  86s   deployment-controller  Scaled up replica set myapp-deployment-64d6bbc45b to 3 from 2
  Normal  ScalingReplicaSet  84s   deployment-controller  Scaled down replica set myapp-deployment-7bf54bc64d to 0 from 1
 ~/k8s/code/K8s/deployments/ [main*]

```

# Update Image in Deployment with record history

```
 ~/k8s/code/K8s/deployments/ [main*] k  set image deployment myapp-deployment nginx=nginx:1.26 --record
Flag --record has been deprecated, --record will be removed in the future
deployment.apps/myapp-deployment image updated
 ~/k8s/code/K8s/deployments/ [main*] k rollout status deployment.apps/myapp-deployment
Waiting for deployment "myapp-deployment" rollout to finish: 2 out of 3 new replicas have been updated...
Waiting for deployment "myapp-deployment" rollout to finish: 2 out of 3 new replicas have been updated...
Waiting for deployment "myapp-deployment" rollout to finish: 2 out of 3 new replicas have been updated...
Waiting for deployment "myapp-deployment" rollout to finish: 1 old replicas are pending termination...
Waiting for deployment "myapp-deployment" rollout to finish: 1 old replicas are pending termination...
deployment "myapp-deployment" successfully rolled out

 ~/k8s/code/K8s/deployments/ [main*] k get pods
NAME                                READY   STATUS    RESTARTS   AGE
myapp-deployment-85dbcb4b4f-glss7   1/1     Running   0          29s
myapp-deployment-85dbcb4b4f-p8mvg   1/1     Running   0          23s
myapp-deployment-85dbcb4b4f-q2zw2   1/1     Running   0          21s
 ~/k8s/code/K8s/deployments/ [main*]


 ~/k8s/code/K8s/deployments/ [main*] k rollout history deployment/myapp-deployment
deployment.apps/myapp-deployment
REVISION  CHANGE-CAUSE
1         kubectl create --filename=deployment.yaml --record=true
2         kubectl edit deployment myapp-deployment --record=true
3         kubectl edit deployment myapp-deployment --record=true
4         kubectl set image deployment myapp-deployment nginx=nginx:1.26 --record=true

 ~/k8s/code/K8s/deployments/ [main*]
```

# Rollback

```
 ~/k8s/code/K8s/deployments/ [main*] k rollout undo deployment myapp-deployment
deployment.apps/myapp-deployment rolled back

 ~/k8s/code/K8s/deployments/ [main*] k rollout status deployment.apps/myapp-deployment
Waiting for deployment "myapp-deployment" rollout to finish: 2 out of 3 new replicas have been updated...
Waiting for deployment "myapp-deployment" rollout to finish: 2 out of 3 new replicas have been updated...
Waiting for deployment "myapp-deployment" rollout to finish: 2 out of 3 new replicas have been updated...
Waiting for deployment "myapp-deployment" rollout to finish: 1 old replicas are pending termination...
Waiting for deployment "myapp-deployment" rollout to finish: 1 old replicas are pending termination...
deployment "myapp-deployment" successfully rolled out

### revison 3 has gone 
 ~/k8s/code/K8s/deployments/ [main*] k rollout history deployment/myapp-deployment
deployment.apps/myapp-deployment
REVISION  CHANGE-CAUSE
1         kubectl create --filename=deployment.yaml --record=true
2         kubectl edit deployment myapp-deployment --record=true
4         kubectl set image deployment myapp-deployment nginx=nginx:1.26 --record=true
5         kubectl edit deployment myapp-deployment --record=true

```