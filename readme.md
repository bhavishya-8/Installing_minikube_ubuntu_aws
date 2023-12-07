Type of instance: Ubuntu
CPU: At least 4 cores



### Installing kubectl

```
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl

```
------------------------------------------------------------------
```
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```
------------------------------------------------------------------
### Install docker
```
sudo apt-get update && sudo apt-get install docker.io -y
```
------------------------------------------------------------------
### Installing minikube
```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/
```
------------------------------------------------------------------
### Installing conntrack
```
sudo apt install conntrack -y
```
------------------------------------------------------------------
### Installing docker-shim.
```
git clone https://github.com/Mirantis/cri-dockerd.git
```

------------------------------------------------------------------
To build for a specific architecture, add ARCH= as an argument, where ARCH is a known build target for golang
To install, on a Linux system that uses systemd, and already has Docker Engine installed
------------------------------------------------------------------

### Run these commands as root
### Install GO###
```
wget https://storage.googleapis.com/golang/getgo/installer_linux
chmod +x ./installer_linux
./installer_linux
source ~/.bash_profile
```

----------------------------------------------
If the above command do not work and give error 400 Bad Request use the below command.
----------------------------------------------

```
apt install golang -y
```

```
cd cri-dockerd
mkdir bin
go get && go build -o bin/cri-dockerd
mkdir -p /usr/local/bin
install -o root -g root -m 0755 bin/cri-dockerd /usr/local/bin/cri-dockerd
cp -a packaging/systemd/* /etc/systemd/system
sed -i -e 's,/usr/bin/cri-dockerd,/usr/local/bin/cri-dockerd,' /etc/systemd/system/cri-docker.service
systemctl daemon-reload
systemctl enable cri-docker.service
systemctl enable --now cri-docker.socket
```

------------------------------------------------------------------

### Installing crictl:

### Using wget:
```
VERSION="v1.24.1"
wget https://github.com/kubernetes-sigs/cri-tools/releases/download/$VERSION/crictl-$VERSION-linux-amd64.tar.gz
sudo tar zxvf crictl-$VERSION-linux-amd64.tar.gz -C /usr/local/bin
rm -f crictl-$VERSION-linux-amd64.tar.gz
```
------------------------------------------------------------------
### Starting minikube
```
minikube start --vm-driver=none
```

------------------------------------------------------------------
If there is an error after running the above command then:
Install containernetworking-plugins from the link given in the error
------------------------------------------------------------------

```
CNI_PLUGIN_VERSION="v1.3.0"
CNI_PLUGIN_TAR="cni-plugins-linux-amd64-$CNI_PLUGIN_VERSION.tgz" # change arch if not on amd64
CNI_PLUGIN_INSTALL_DIR="/opt/cni/bin"

curl -LO "https://github.com/containernetworking/plugins/releases/download/$CNI_PLUGIN_VERSION/$CNI_PLUGIN_TAR"
sudo mkdir -p "$CNI_PLUGIN_INSTALL_DIR"
sudo tar -xf "$CNI_PLUGIN_TAR" -C "$CNI_PLUGIN_INSTALL_DIR"
rm "$CNI_PLUGIN_TAR"
```
-----------------------------------------------------------------
Now again try the command
-----------------------------------------------------------------

```
minikube start --vm-driver=none
```
