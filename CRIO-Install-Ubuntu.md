Update System

```
apt update && sudo apt upgrade
```

Ubuntu 22.04/20.04:

```
OS=xUbuntu_20.04
CRIO_VERSION=1.23
echo "deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/ /"|sudo tee /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list
echo "deb http://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable:/cri-o:/$CRIO_VERSION/$OS/ /"|sudo tee /etc/apt/sources.list.d/devel:kubic:libcontainers:stable:cri-o:$CRIO_VERSION.list
```

Ubuntu 18.04:

```
OS=xUbuntu_18.04
CRIO_VERSION=1.23
echo "deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/ /"|sudo tee /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list
echo "deb http://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable:/cri-o:/$CRIO_VERSION/$OS/ /"|sudo tee /etc/apt/sources.list.d/devel:kubic:libcontainers:stable:cri-o:$CRIO_VERSION.list
```

import GPG key

```text
curl -L https://download.opensuse.org/repositories/devel:kubic:libcontainers:stable:cri-o:$CRIO_VERSION/$OS/Release.key | sudo apt-key add -
curl -L https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/Release.key | sudo apt-key add -
```

# Install CRI-O on Ubuntu22.04|20.04|18.04

```
sudo apt update
sudo apt install cri-o cri-o-runc
```

Checking the version of CRI-O installed on Ubuntu:

```text
apt show cri-o
```

Start and enable crio service:

```text
sudo systemctl enable crio.service
sudo systemctl start crio.service
```

Status

```text
systemctl status crio
```
</details>
# Using CRI-O on Ubuntu22.04|20.04|18.04
 
```text
sudo apt install cri-tools
sudo crictl info
sudo crictl pull nginx
sudo crictl pull hello-world
sudo crictl images
```