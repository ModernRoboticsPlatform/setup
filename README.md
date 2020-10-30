# setup

### Enable cgroup_memory
```
$ sudo vi /boot/firmware/cmdline.txt
cgroup_enable=memory swapaccount=1 cgroup_memory=1 cgroup_enable=cpuset
```

### Let iptables see bridged traffic.
```
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sudo sysctl --system
```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
### Install dependencies
```
$ sudo apt-get update && sudo apt-get install -y \
    python-dev \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common \
    git \
    vim \
    netstat
```

### Install Docker
```
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

$ sudo add-apt-repository \
   "deb [arch=arm64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"

$ sudo apt-get install docker-ce docker-ce-cli containerd.io conntrack 

$ sudo usermod -aG docker $USER


# Setup docker ops
cat > /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF
```

### Install helm
```
$ curl https://baltocdn.com/helm/signing.asc | sudo apt-key add -
$ sudo apt-get install apt-transport-https --yes
$ echo "deb https://baltocdn.com/helm/stable/debian/ all main" | sudo tee /etc/apt/sources.list.d/helm-stable-debian.list
$ sudo apt-get update
$ sudo apt-get install helm
```


### Install Kong
```
$ helm repo add kong https://charts.konghq.com
$ helm install kong/kong --generate-name --set ingressController.installCRDs=false
```

### Install mini-kube
```
$ curl -LO https://storage.googleapis.com/minikube/releases/v1.6.2/minikube-linux-arm64 

$ chmod +x minikube-linux-arm

$ sudo ./minikube-linux-arm start --vm-driver=none
```
