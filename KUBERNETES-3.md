#### Persistent Volume
> Common External Storage for clusters

##### Pre-requisites
on master create directory or a new disk and perform other requirements
```
mkdir /ibmdata
chmod 777 /ibmdata/
yum install nfs* -y
systemctl enable nfs-server
systemctl start nfs-server
```

add following line in /etc/exports
```
/ibm_data *(rw,sync)
```

restart and check if successfully exported
```
systemctl restart nfs-server 
exportfs -av 
```

on worker nodes
```
yum install nfs-utils -y
showmount -e 192.168.0.130
```

```
mkdir /ibmdata
mount -t nfs 192.168.0.130:/ibmdata /ibmdata
```

after mounting successfully the files in /ibmdata should be in sync

##### Creating PV, PVC and Pod

create yaml config for persistent volume
> [pv1.yaml](https://gitlab-nht.stackroute.in/Laribok.Syiemlieh/openstack-notes/-/blob/master/pv1.yaml)

```
kubectl create -f pv1.yaml
kubectl get pv
```

create pvc config
> [pvc1.yaml](https://gitlab-nht.stackroute.in/Laribok.Syiemlieh/openstack-notes/-/blob/master/pvc1.yaml)
```
kubectl create -f pvc1.yaml
kubectl get pvc
kubectl get pv
```

create pod config
> [pod1.yaml](https://gitlab-nht.stackroute.in/Laribok.Syiemlieh/openstack-notes/-/blob/master/pod1.yaml)
```
kubectl describe pod/ibm
```

##### Demonstration
on master or any worker
```
curl 10.36.0.1
```

override index.html by using the following commands 
```
echo "Data is coming from pv1/pvc1">/ibmdata/index.html
curl 10.36.0.1
```
