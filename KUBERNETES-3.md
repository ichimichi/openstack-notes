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

#### RBAC
> Role Based Access Control

Authentication
> userid/password, certificates SSL/TLS config file

Authorization
> RBAC performs authorizaton, e.g can user create pod?

Admission Control
>

Admission Controller
>

```
cat /root/.kube/config
```
or
```
kubectl config view
```

##### Demonstration

create a user
```
useradd ibm
echo 123 | passwd --stdin ibm
```

cd to ibm user directory
```
cd /home/ibm
```

generate key using openssl
```
openssl genrsa -out ibm.key 2048 
```

create CSR using key
```
openssl req -new -key ibm.key -out ibm.csr -subj "/CN=ibm"
```

generate certificate
```
openssl x509 -req -in ibm.csr -CA /etc/kubernetes/pki/ca.crt -CAkey /etc/kubernetes/pki/ca.key -CAcreateserial -out ibm.crt -days 500
```

```
mkdir .certs
mv ibm.* .certs/
cd .certs
```

```
kubectl config set-credentials ibm --client-certificate=/home/ibm/.certs/ibm.crt --client-key=/home/ibm/.certs/ibm.key
kubectl config view
```

Namespace
```
kubectl create namespace google
kubectl get ns
kubectl create deployment google --image=nginx -n google
kubectl get pods -n google
```

Context
```
kubectl config set-context ibm-context --cluster=kubernetes --namespace=google --user=ibm
kubectl config get-contexts
kubectl --context=ibm-context get pods
```
> Error from server (Forbidden): pods is forbidden: User "ibm" cannot list resource "pods" in API group "" in the namespace "google"

need RBAC

* verbs
* resources
* subjects

> **role:** defining action on rersources
>
> **role binding:** binding subject to roles

> [role.yaml](https://gitlab-nht.stackroute.in/Laribok.Syiemlieh/openstack-notes/-/blob/master/role.yaml)

```
kubectl apply -f role.yaml
kubectl get role -n google
kubectl describe role developer -n google
```

> [rolebind.yaml](https://gitlab-nht.stackroute.in/Laribok.Syiemlieh/openstack-notes/-/blob/master/rolebind.yaml)

```
kubectl apply -f rolebind.yaml
kubectl get rolebinding -n google
kubectl describe rolebinding developer-role-binding -n google
kubectl --context=ibm-context get pods
```
