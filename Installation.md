# Kubernetes Installation using Kubeadm

## Installing kubeadm

### Prerequisites

- One or more machines running one of:
  - Ubuntu 16.04+
  - Debian 9+
  - CentOS 7
  - Red Hat Enterprise Linux (RHEL) 7

- 2 GB or more of RAM per machine (any less will leave little room for your apps)
- 2 CPUs or more
- Full network connectivity between all machines in the cluster (public or private network is fine)

**Docker Support has been removed in latest version of Kubernetes**

# [Don't Panic: Kubernetes and Docker](https://kubernetes.io/blog/2020/12/02/dont-panic-kubernetes-and-docker/)

# Install CRI-O Container Runtime on Ubuntu 22.04|20.04|18.04 [Refer here](https://computingforgeeks.com/install-cri-o-container-runtime-on-ubuntu-linux/)

# Install CRI-O Container Runtime on CentOS 8 / CentOS 7 [Refer here](https://computingforgeeks.com/install-cri-o-container-runtime-on-centos-linux/)

_________

# Install Kubeadm Kubelet kubectl On all Nodes

[Official Kubernetes Documentation](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)


<details>
 <summary>Installation on Debian-Based Distrubutions</summary>
 <br>
Update the apt package index and install packages needed to use the Kubernetes apt repository:

```text
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl
```
Download the Google Cloud public signing key:
```
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
```
Add the Kubernetes apt repository:
```
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
```
Update apt package index, install kubelet, kubeadm and kubectl, and pin their version:
```
sudo apt-get update
# For Specific Version use below commands
apt list -a kubeadm
apt install -y kubeadm=1.22.11-00 kubelet=1.22.11-00 kubectl=1.22.11-00
# for latest version use below commands

sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

# Forwarding IPv4 and letting iptables see bridged traffic 
```
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter

# sysctl params required by setup, params persist across reboots
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

# Apply sysctl params without reboot
sudo sysctl --system
```
</details>

_________

<details>
<summary>Installation of RPM based OS</summary>
<br>
Configuring Kubernetes cluster with kubeadm:

## Configure Repository for Kubeadm

```sh
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF

# Set SELinux in permissive mode (effectively disabling it)
setenforce 0

# If you want to install specific version of kubernetes 

yum list -a kubeadm

yum install -y kubeadm=1.22.11-00 kubelet=1.22.11-00 kubectl=1.22.11-00

yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
systemctl enable --now kubelet

Some users on RHEL/CentOS 7 have reported issues with traffic being routed incorrectly due to iptables being bypassed. You should ensure net.bridge.bridge-nf-call-iptables is set to 1 in your sysctl config, e.g

cat <<EOF >  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

sysctl --system
```
</details>
_________

## Using kubeadm to Create a Cluster

Installing kubeadm on your hosts:(Master)

## Initializing your control-plane node

we will be installing calico network

[Calico Installation](https://projectcalico.docs.tigera.io/getting-started/kubernetes/quickstart)

```sh
kubeadm init --pod-network-cidr=192.168.0.0/16

Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You can now join any number of machines by running the following on each node
as root:

  kubeadm join <control-plane-host>:<control-plane-port> --token <token> --discovery-token-ca-cert-hash sha256:<hash>

```

**Note: Copy the Join Token and run on all the nodes**

## Installing a pod network add-on

```sh
kubectl create -f https://projectcalico.docs.tigera.io/manifests/tigera-operator.yaml

kubectl create -f https://projectcalico.docs.tigera.io/manifests/custom-resources.yaml

watch kubectl get pods -n calico-system

kubectl get nodes -o wide

```

```kubectl cluster-info```

```kubectl get pods -n kube-system```

```kubectl get nodes```
