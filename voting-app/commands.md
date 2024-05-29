

# Execution flow
```
 ~/k8s/code/K8s/voting-app/ [main*] ls -lrt
total 72
-rw-r--r--@ 1 kiruviji  staff  526 May 29 20:50 voting-app-deploy.yaml
-rw-r--r--@ 1 kiruviji  staff  489 May 29 20:53 redis-app-deploy.yaml
-rw-r--r--@ 1 kiruviji  staff  479 May 29 20:55 worker-app-deploy.yaml
-rw-r--r--@ 1 kiruviji  staff  528 May 29 20:57 result-app-deploy.yaml
-rw-r--r--@ 1 kiruviji  staff  661 May 29 21:09 postgres-app-deploy.yaml
-rw-r--r--@ 1 kiruviji  staff  227 May 29 21:12 redis-app-service.yaml
-rw-r--r--@ 1 kiruviji  staff  230 May 29 21:13 postgres-app-service.yaml
-rw-r--r--@ 1 kiruviji  staff  273 May 29 21:16 result-app-service.yaml
-rw-r--r--@ 1 kiruviji  staff  273 May 29 21:16 voting-app-service.yaml
 ~/k8s/code/K8s/voting-app/ [main*] k get pods
No resources found in default namespace.
 ~/k8s/code/K8s/voting-app/ [main*] k get svc
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   2d4h
 ~/k8s/code/K8s/voting-app/ [main*] k create -f voting-app-deploy.yaml 
deployment.apps/voting-app-deploy created
 ~/k8s/code/K8s/voting-app/ [main*] k create -f voting-app-service.yaml 
service/voting-service created
 ~/k8s/code/K8s/voting-app/ [main*] k get all
NAME                                    READY   STATUS    RESTARTS   AGE
pod/voting-app-deploy-96f58547f-zqf6m   1/1     Running   0          20s

NAME                     TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
service/kubernetes       ClusterIP   10.96.0.1      <none>        443/TCP        2d4h
service/voting-service   NodePort    10.96.105.29   <none>        80:30004/TCP   11s

NAME                                READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/voting-app-deploy   1/1     1            1           20s

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/voting-app-deploy-96f58547f   1         1         1       20s
 ~/k8s/code/K8s/voting-app/ [main*]  
 ~/k8s/code/K8s/voting-app/ [main*] 
 ~/k8s/code/K8s/voting-app/ [main*] k create -f redis-app-deploy.yaml 
deployment.apps/redis-app-deplpy created
 ~/k8s/code/K8s/voting-app/ [main*] k create -f redis-app-service.yaml 
service/redis created
 ~/k8s/code/K8s/voting-app/ [main*] k create -f postgres-app-deploy.yaml 
deployment.apps/postgres-app-deploy created
 ~/k8s/code/K8s/voting-app/ [main*] k create -f postgres-app-service.yaml 
service/db created
 ~/k8s/code/K8s/voting-app/ [main*] k get deploy
NAME                  READY   UP-TO-DATE   AVAILABLE   AGE
postgres-app-deploy   1/1     1            1           25s
redis-app-deplpy      1/1     1            1           45s
voting-app-deploy     1/1     1            1           89s
 ~/k8s/code/K8s/voting-app/ [main*] k get pods
NAME                                   READY   STATUS    RESTARTS   AGE
postgres-app-deploy-68c8bf5cc6-bwmwb   1/1     Running   0          33s
redis-app-deplpy-84499cc996-wx5lq      1/1     Running   0          53s
voting-app-deploy-96f58547f-zqf6m      1/1     Running   0          97s
 ~/k8s/code/K8s/voting-app/ [main*] k get svc
NAME             TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
db               ClusterIP   10.96.106.157    <none>        5432/TCP       29s
kubernetes       ClusterIP   10.96.0.1        <none>        443/TCP        2d4h
redis            ClusterIP   10.105.172.163   <none>        6379/TCP       45s
voting-service   NodePort    10.96.105.29     <none>        80:30004/TCP   92s
 ~/k8s/code/K8s/voting-app/ [main*] 
 ~/k8s/code/K8s/voting-app/ [main*] 
 ~/k8s/code/K8s/voting-app/ [main*] k create -f worker-app-deploy.yaml 
deployment.apps/worker-app-deploy created
 ~/k8s/code/K8s/voting-app/ [main*] k get pods, svc
error: arguments in resource/name form must have a single resource and name
 ~/k8s/code/K8s/voting-app/ [main*] k get pods,svc 
NAME                                       READY   STATUS    RESTARTS   AGE
pod/postgres-app-deploy-68c8bf5cc6-bwmwb   1/1     Running   0          86s
pod/redis-app-deplpy-84499cc996-wx5lq      1/1     Running   0          106s
pod/voting-app-deploy-96f58547f-zqf6m      1/1     Running   0          2m30s
pod/worker-app-deploy-54575bd48c-t8zlg     1/1     Running   0          35s

NAME                     TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
service/db               ClusterIP   10.96.106.157    <none>        5432/TCP       78s
service/kubernetes       ClusterIP   10.96.0.1        <none>        443/TCP        2d4h
service/redis            ClusterIP   10.105.172.163   <none>        6379/TCP       94s
service/voting-service   NodePort    10.96.105.29     <none>        80:30004/TCP   2m21s
 ~/k8s/code/K8s/voting-app/ [main*] k create -f result-app-deploy.yaml 
deployment.apps/result-app-deploy created
 ~/k8s/code/K8s/voting-app/ [main*] k create -f result-app-service.yaml
```