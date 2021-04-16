#### IBM CLOUD CLI
> [Guide](https://cloud.ibm.com/docs/openshift?topic=openshift-openshift-cli)

```
wget https://clis.cloud.ibm.com/download/bluemix-cli/latest/linux64
mv linux64 linux64.tar.gz
tar xvf linux64.tar.gz
sudo ./Bluemix_CLI/install
ibmcloud login
```


```
# oc login --token=4uXPrwZfdMtIJvGPNNfDa_ZVscxpfGSjqORzQBNpe04 --server=https://c101-e.us-south.containers.cloud.ibm.com:30023 
oc login -u lari
# oc project lari
oc adm policy add-scc-to-user anyuid -z default
```

#### Deploying image
```
oc new-app --name=ibm1 docker.io/nginx
```

#### Deployment vs Deployment Config
```
oc get deployment
```
> No resources found

```
oc get dc -o wide
oc get pods
oc get services
oc expose svc/ibm1
oc get route

```
> creates deployment config and 2 pods, one for build and one for deployment

#### Scaling
```
oc scale dc ibm2 --replicas=3
```

#### Deploying DB
```
oc new-app --name=db2 docker.io/mysql -e MYSQL_ROOT_PASSWORD=123
```

### FullStack Application Deployment with web hooks

##### Web Hook
> [Guide](https://developer.ibm.com/technologies/containers/tutorials/github-webhook-triggers-openshift/)

##### Frontend
> [repo](https://github.com/ichimichi/ibm8)

#### Backend
> [repo](https://github.com/ichimichi/backend)
