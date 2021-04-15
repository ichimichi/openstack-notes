### Openshift architechture
>

### RH Openshift Container Platform RHOCP
>

#### OKD installation 9 GB
> create aws ec2 instance with CentOS7, 2/4 cores and 8GB ram

```
sudo yum -y update
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install -y  docker-ce docker-ce-cli containerd.io
sudo mkdir /etc/docker /etc/containers
```

```
sudo tee /etc/containers/registries.conf<<EOF
[registries.insecure]
registries = ['172.30.0.0/16']
EOF
```

```
sudo tee /etc/docker/daemon.json<<EOF
{
   "insecure-registries": [
     "172.30.0.0/16"
   ]
}
EOF
```

```
sudo systemctl daemon-reload
sudo systemctl restart docker
sudo systemctl enable docker
```

```
echo "net.ipv4.ip_forward = 1" | sudo tee -a /etc/sysctl.conf
```
> net.ipv4.ip_forward = 1   

```
sudo sysctl -p
```

```
yum install wget -y
wget https://github.com/openshift/origin/releases/download/v3.11.0/openshift-origin-client-tools-v3.11.0-0cbc58b-linux-64bit.tar.gz
tar xvf openshift-origin-client-tools*.tar.gz
```

```
cd openshift-origin-client*/
cp oc kubectl  /usr/bin/
```

```
oc version
```
> oc v3.11.0+0cbc58b
> 
> kubernetes v1.11.0+d4cacc0 
>
> features: Basic-Auth GSSAPI Kerberos SPNEGO 

```
oc cluster up --public-hostname=aws_public_IP
```
> OpenShift server started.
>
> The server is accessible via web console at:
>
>    https://aws_public_IP:8443
>
> You are logged in as: 
>
>    User: developer Password: <any value> 
>
> To login as administrator: 
>
>    oc login -u system:admin 

open https://aws_public_IP:8443/console on browser and login as developer

```
oc login -u system:admin 
oc adm policy add-scc-to-user anyuid -z default
```
> scc "anyuid" added to: ["system:serviceaccount:myproject:default"]  

```
oc get project
oc get pods -o wide
oc exec -it 
```

### Openshift Enterprise Installation
>  AWS - 4 machines 

### Deploy applications - docker, github, source code directly

### AutoScaling, Monitoring, Storage
