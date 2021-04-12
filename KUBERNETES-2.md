### Imperative
```
kubectl run pod2 --image=nginx  
curl 10.44.0.1                                         
```

### Declarative
mypod.yaml
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
