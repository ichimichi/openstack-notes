cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF

yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes

systemctl enable --now kubelet

cat <<EOF >/etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

sysctl --system

lsmod | grep br_netfilter
systemctl daemon-reload
systemctl restart kubelet


(on master) kubeadm init [only for local] --apiserver-advertise-address=

(on master) mkdir -p $HOME/.kube
(on master) cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
(on master) chown $(id -u):$(id -g) $HOME/.kube/config

[check nodes](on master) kubectl get nodea
[check pods](on master) kubectl get pods -n kube-system

[if coredns pending](on master) kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 | tr -d '\n')"




--------------------------
To start using your cluster, you need to run the following as a regular user: 
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
Alternatively, if you are the root user, you can run: 
export KUBECONFIG=/etc/kubernetes/admin.conf
You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at: 
https://kubernetes.io/docs/concepts/cluster-administration/addons/Then you can join any number of worker nodes by running the following on each as root:

[if failed to join](on worker) kubeadm reset
kubeadm join 192.168.0.130:6443 --token ihf2cq.slhg3r6hk178o1j3 --discovery-token-ca-cert-hash sha256:ae51ff3a86eec452ffebc90717081e358fae4400b8870eb83ca329490a21eb4f

--------------------------
to clean up master node
kubeadm reset
rm -fr /root/.kube
kubeadm init
