### Imperative
```
kubectl run pod2 --image=nginx  
curl 10.44.0.1                                         
```

### Declarative
> mypod.yaml
```
apiVersion: v1
kind: Pod
metadata:
    name: idli
    labels:
        dish: vadapav
    annotations:
        refimage: nginxv1.0
spec:
    containers:
    - name: c1
      image: nginx
      ports:
      - containerPort: 80
```

```
kubectl explain pod
```

```
kubectl create -f mypod.yaml  
```

### Modifying delpoyed pods
```
kubectl edit idli
```
or
edit yaml file
and execute
```
kubectl apply -f mypod.yaml
```

### Deleting deployed pods
```
kubectl delete pod idli
```

### Scale up/down applications, Self Healing

##### kind: Deployment
creates pod with replica sets
```
kubectl create deployment app1 --image=httpd
kubectl get deployments -o wide
kubectl get pods -o wide
kubectl get rs
```

##### Scaling
```
kubectl scale deployment/app1 --replicas=4
kubectl get pods -o wide
kubectl get rs
```

##### Demonstration of Self Healing
> watch kubectl pods on a new session
```
watch kubectl get pods -o wide
```

```
kubectl delete pod app1-sajdhkdfs
```

### Access Application
> ClusterIP, NodePort & LoadBalancer

##### NodePort
accessible externally, Round Robin method
```
kubectl expose deployment/app1 --type=NodePort --port=80
kubectl get service
```

```
kubectl exec -it app1-sdsdf bash
```

##### ClusterIP
accessible internally only, Round Robin method
```
kubectl expose deployment/app1 --type=ClusterIP --port=80
kubectl get service
```

##### LoadBalancer
> kubernetes should be cloud native
Load Balancer is required to perform this, otherwise external
ip will be in pending state
```
kubectl expose deployment/app1 --type=LoadBalancer --port=80
kubectl get service
```

### Rolling Out and Rolling Back
to check rollout history
```
kubectl rollout history deployment/app3
```
##### Demonstration : Rolling out new version
> create new deployment with 4 replicas and expose with type nodeport or loadbalancer
```
kubectl create deployment app3 --image=docker.io/kharatramesh/k8s_rollaback:webv1.0
kubectl expose deployment/app3 --type=NodePort --port=80
kubectl scale deployment/app3 --replicas=4
```

obtain container name, which will be required during rolling out
```
kubectl describe deployment/app3
```

Rolling out new version
```
kubectl --record deployment.apps/app3 set image deployment.v1.apps/app3 k8s-rollaback-7n5x4=docker.io/kharatramesh/k8s_rollaback:webv2.0
```

##### Demonstration : Rolling back to older version
> check rollout history for revision number
```
kubectl rollout undo deployment/app3 --to-revision=2
```

