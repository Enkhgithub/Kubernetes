# Kubernetes

<copy-button sadsadsadsfaasfsa >  </copy-button>
##	CRI-O	


export OS="xUbuntu_22.04"
export VERSION="1.27"

##
  /echo "deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/ /" > /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list
echo "deb http://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable:/cri-o:/$VERSION/$OS/ /" > /etc/apt/sources.list.d/devel:kubic:libcontainers:stable:cri-o:$VERSION.list

curl -L https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable:/cri-o:/$VERSION/$OS/Release.key | apt-key add -
curl -L https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/Release.key | apt-key add -

apt-get update
apt-get install -y cri-o cri-o-runc


######		Kubernetes		######

These steps have to be performed on both nodes.

modprobe br_netfilter
echo 1 > /proc/sys/net/bridge/bridge-nf-call-iptables
echo 1 > /proc/sys/net/ipv4/ip_forward

$  modprobe br_netfilter

set net.bridge.bridge-nf-call-iptables to 1:

cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

sudo sysctl --system

The container runtime has already been installed on both nodes, so you may skip this step.
Install kubeadm, kubectl and kubelet on all nodes:

sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl



curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.27/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.27/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update

# To see the new version labels

sudo apt-cache madison kubeadm

swapoff -a


sudo apt-get install -y kubelet kubeadm kubectl

sudo apt-mark hold kubelet kubeadm kubectl


IP_ADDR=$(ip addr show ens160 | grep -oP '(?<=inet\s)\d+(\.\d+){3}')

kubeadm init --apiserver-cert-extra-sans=controlplane --apiserver-advertise-address $IP_ADDR --ignore-preflight-errors=all --pod-network-cidr=10.244.0.0/16
kubeadm init --apiserver-cert-extra-sans=controlplane --apiserver-advertise-address 192.168.100.160 --pod-network-cidr=10.244.0.0/16


Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.23.108.8:6443 --token udz7gw.ock7lve14hw0cdpp \
        --discovery-token-ca-cert-hash sha256:e7755ede5d6f0bae08bca4e13ccce8923995245ca68bba5f7ccc53c9b9728cdb 

root@controlplane:~> mkdir -p $HOME/.kube
root@controlplane:~> sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
root@controlplane:~> sudo chown $(id -u):$(id -g) $HOME/.kube/config

 kubeadm token create --print-join-command

kubeadm join 192.23.108.8:6443 --token thubfg.2zq20f5ttooz27op --discovery-token-ca-cert-hash sha256:e7755ede5d6f0bae08bca4e13ccce8923995245ca68bba5f7ccc53c9b9728cdb



root@node01:~> kubeadm join 192.23.108.8:6443 --token thubfg.2zq20f5ttooz27op --discovery-token-ca-cert-hash sha256:e7755ede5d6f0bae08bca4e13ccce8923995245ca68bba5f7ccc53c9b9728cdb 
[preflight] Running pre-flight checks
[preflight] Reading configuration from the cluster...
[preflight] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -o yaml'
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Starting the kubelet
[kubelet-start] Waiting for the kubelet to perform the TLS Bootstrap...

This node has joined the cluster:
* Certificate signing request was sent to apiserver and a response was received.
* The Kubelet was informed of the new secure connection details.

Run 'kubectl get nodes' on the control-plane to see this node join the cluster.

curl -LO https://raw.githubusercontent.com/flannel-io/flannel/v0.20.2/Documentation/kube-flannel.yml


  args:
  - --ip-masq
  - --kube-subnet-mgr


kubectl apply -f kube-flannel.yml



curl -L https://download.opensuse.org/repositories/devel:kubic:libcontainers:stable:cri-o:$VERSION/$OS/Release.key | sudo apt-key --keyring /etc/apt/trusted.gpg.d/libcontainers.gpg add -
curl -L https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/Release.key | sudo apt-key --keyring /etc/apt/trusted.gpg.d/libcontainers.gpg add -


kubeadm join 192.168.1.21:6443 --token lk4rh6.5pj2grxlhlye4xjm --discovery-token-ca-cert-hash sha256:354df63349a888d1c6d0b8a06b310b1deb490ee7be6b6b41b2f81b7a0f44e538



kubeadm join 172.16.101.21:6443 --token mx5n7u.z0d9jfbdg4rebk9p --discovery-token-ca-cert-hash sha256:0bc2f0f4746bf7c64ae790b8455c600599448d6e4cc9b03d661d3ce7634329d4



kubeadm join 172.16.101.41:6443 --token dbl9yc.jmg4keymd7fage9b \
        --discovery-token-ca-cert-hash sha256:c32c1547c0f714dd2aad2732001d5b1409efce4bde85a5a8f29a7806e2136a6d



kubeadm join 192.168.1.40:6443 --token 2qatms.3c2vdr215kzq7asu \
        --discovery-token-ca-cert-hash sha256:19ccf2654e1e0e429c93d8cfb45a2d338c91611911e264c4950c2a74faf16c20

kubeadm join 192.168.100.160:6443 --token i4rxad.t9lrgc7k83wayrx4 \
        --discovery-token-ca-cert-hash sha256:66f0049f544c282d32042497d9abfc9cf1b4cd296d78a7fa6b85b1a31bdcc929


######	CRI-O	######


export OS="xUbuntu_22.04"
export VERSION="1.27"


echo "deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/ /" > /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list
echo "deb http://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable:/cri-o:/$VERSION/$OS/ /" > /etc/apt/sources.list.d/devel:kubic:libcontainers:stable:cri-o:$VERSION.list

curl -L https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable:/cri-o:/$VERSION/$OS/Release.key | apt-key add -
curl -L https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/Release.key | apt-key add -

apt-get update
apt-get install -y cri-o cri-o-runc


######		Kubernetes		######

These steps have to be performed on both nodes.

modprobe br_netfilter
echo 1 > /proc/sys/net/bridge/bridge-nf-call-iptables
echo 1 > /proc/sys/net/ipv4/ip_forward

$  modprobe br_netfilter

set net.bridge.bridge-nf-call-iptables to 1:

cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

sudo sysctl --system

The container runtime has already been installed on both nodes, so you may skip this step.
Install kubeadm, kubectl and kubelet on all nodes:

sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl



curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.27/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.27/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update

# To see the new version labels

sudo apt-cache madison kubeadm

swapoff -a


sudo apt-get install -y kubelet kubeadm kubectl

sudo apt-mark hold kubelet kubeadm kubectl


IP_ADDR=$(ip addr show ens160 | grep -oP '(?<=inet\s)\d+(\.\d+){3}')

kubeadm init --apiserver-cert-extra-sans=controlplane --apiserver-advertise-address $IP_ADDR --ignore-preflight-errors=all --pod-network-cidr=10.244.0.0/16
kubeadm init --apiserver-cert-extra-sans=controlplane --apiserver-advertise-address 192.168.100.160 --pod-network-cidr=10.244.0.0/16


Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.23.108.8:6443 --token udz7gw.ock7lve14hw0cdpp \
        --discovery-token-ca-cert-hash sha256:e7755ede5d6f0bae08bca4e13ccce8923995245ca68bba5f7ccc53c9b9728cdb 

root@controlplane:~> mkdir -p $HOME/.kube
root@controlplane:~> sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
root@controlplane:~> sudo chown $(id -u):$(id -g) $HOME/.kube/config

 kubeadm token create --print-join-command

kubeadm join 192.23.108.8:6443 --token thubfg.2zq20f5ttooz27op --discovery-token-ca-cert-hash sha256:e7755ede5d6f0bae08bca4e13ccce8923995245ca68bba5f7ccc53c9b9728cdb



root@node01:~> kubeadm join 192.23.108.8:6443 --token thubfg.2zq20f5ttooz27op --discovery-token-ca-cert-hash sha256:e7755ede5d6f0bae08bca4e13ccce8923995245ca68bba5f7ccc53c9b9728cdb 
[preflight] Running pre-flight checks
[preflight] Reading configuration from the cluster...
[preflight] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -o yaml'
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Starting the kubelet
[kubelet-start] Waiting for the kubelet to perform the TLS Bootstrap...

This node has joined the cluster:
* Certificate signing request was sent to apiserver and a response was received.
* The Kubelet was informed of the new secure connection details.

Run 'kubectl get nodes' on the control-plane to see this node join the cluster.

curl -LO https://raw.githubusercontent.com/flannel-io/flannel/v0.20.2/Documentation/kube-flannel.yml


  args:
  - --ip-masq
  - --kube-subnet-mgr


kubectl apply -f kube-flannel.yml



curl -L https://download.opensuse.org/repositories/devel:kubic:libcontainers:stable:cri-o:$VERSION/$OS/Release.key | sudo apt-key --keyring /etc/apt/trusted.gpg.d/libcontainers.gpg add -
curl -L https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/Release.key | sudo apt-key --keyring /etc/apt/trusted.gpg.d/libcontainers.gpg add -


kubeadm join 192.168.1.21:6443 --token lk4rh6.5pj2grxlhlye4xjm --discovery-token-ca-cert-hash sha256:354df63349a888d1c6d0b8a06b310b1deb490ee7be6b6b41b2f81b7a0f44e538



kubeadm join 172.16.101.21:6443 --token mx5n7u.z0d9jfbdg4rebk9p --discovery-token-ca-cert-hash sha256:0bc2f0f4746bf7c64ae790b8455c600599448d6e4cc9b03d661d3ce7634329d4



kubeadm join 172.16.101.41:6443 --token dbl9yc.jmg4keymd7fage9b \
        --discovery-token-ca-cert-hash sha256:c32c1547c0f714dd2aad2732001d5b1409efce4bde85a5a8f29a7806e2136a6d



kubeadm join 192.168.1.40:6443 --token 2qatms.3c2vdr215kzq7asu \
        --discovery-token-ca-cert-hash sha256:19ccf2654e1e0e429c93d8cfb45a2d338c91611911e264c4950c2a74faf16c20

kubeadm join 192.168.100.160:6443 --token i4rxad.t9lrgc7k83wayrx4 \
        --discovery-token-ca-cert-hash sha256:66f0049f544c282d32042497d9abfc9cf1b4cd296d78a7fa6b85b1a31bdcc929


######	CRI-O	######


export OS="xUbuntu_22.04"
export VERSION="1.27"


echo "deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/ /" > /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list
echo "deb http://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable:/cri-o:/$VERSION/$OS/ /" > /etc/apt/sources.list.d/devel:kubic:libcontainers:stable:cri-o:$VERSION.list

curl -L https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable:/cri-o:/$VERSION/$OS/Release.key | apt-key add -
curl -L https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/Release.key | apt-key add -

apt-get update
apt-get install -y cri-o cri-o-runc


######		Kubernetes		######

These steps have to be performed on both nodes.

modprobe br_netfilter
echo 1 > /proc/sys/net/bridge/bridge-nf-call-iptables
echo 1 > /proc/sys/net/ipv4/ip_forward

$  modprobe br_netfilter

set net.bridge.bridge-nf-call-iptables to 1:

cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

sudo sysctl --system

The container runtime has already been installed on both nodes, so you may skip this step.
Install kubeadm, kubectl and kubelet on all nodes:

sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl



curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.27/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.27/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update

# To see the new version labels

sudo apt-cache madison kubeadm

swapoff -a


sudo apt-get install -y kubelet kubeadm kubectl

sudo apt-mark hold kubelet kubeadm kubectl


IP_ADDR=$(ip addr show ens160 | grep -oP '(?<=inet\s)\d+(\.\d+){3}')

kubeadm init --apiserver-cert-extra-sans=controlplane --apiserver-advertise-address $IP_ADDR --ignore-preflight-errors=all --pod-network-cidr=10.244.0.0/16
kubeadm init --apiserver-cert-extra-sans=controlplane --apiserver-advertise-address 192.168.100.160 --pod-network-cidr=10.244.0.0/16


Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.23.108.8:6443 --token udz7gw.ock7lve14hw0cdpp \
        --discovery-token-ca-cert-hash sha256:e7755ede5d6f0bae08bca4e13ccce8923995245ca68bba5f7ccc53c9b9728cdb 

root@controlplane:~> mkdir -p $HOME/.kube
root@controlplane:~> sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
root@controlplane:~> sudo chown $(id -u):$(id -g) $HOME/.kube/config

 kubeadm token create --print-join-command

kubeadm join 192.23.108.8:6443 --token thubfg.2zq20f5ttooz27op --discovery-token-ca-cert-hash sha256:e7755ede5d6f0bae08bca4e13ccce8923995245ca68bba5f7ccc53c9b9728cdb



root@node01:~> kubeadm join 192.23.108.8:6443 --token thubfg.2zq20f5ttooz27op --discovery-token-ca-cert-hash sha256:e7755ede5d6f0bae08bca4e13ccce8923995245ca68bba5f7ccc53c9b9728cdb 
[preflight] Running pre-flight checks
[preflight] Reading configuration from the cluster...
[preflight] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -o yaml'
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Starting the kubelet
[kubelet-start] Waiting for the kubelet to perform the TLS Bootstrap...

This node has joined the cluster:
* Certificate signing request was sent to apiserver and a response was received.
* The Kubelet was informed of the new secure connection details.

Run 'kubectl get nodes' on the control-plane to see this node join the cluster.

curl -LO https://raw.githubusercontent.com/flannel-io/flannel/v0.20.2/Documentation/kube-flannel.yml


  args:
  - --ip-masq
  - --kube-subnet-mgr


kubectl apply -f kube-flannel.yml



curl -L https://download.opensuse.org/repositories/devel:kubic:libcontainers:stable:cri-o:$VERSION/$OS/Release.key | sudo apt-key --keyring /etc/apt/trusted.gpg.d/libcontainers.gpg add -
curl -L https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/Release.key | sudo apt-key --keyring /etc/apt/trusted.gpg.d/libcontainers.gpg add -


kubeadm join 192.168.1.21:6443 --token lk4rh6.5pj2grxlhlye4xjm --discovery-token-ca-cert-hash sha256:354df63349a888d1c6d0b8a06b310b1deb490ee7be6b6b41b2f81b7a0f44e538



kubeadm join 172.16.101.21:6443 --token mx5n7u.z0d9jfbdg4rebk9p --discovery-token-ca-cert-hash sha256:0bc2f0f4746bf7c64ae790b8455c600599448d6e4cc9b03d661d3ce7634329d4



kubeadm join 172.16.101.41:6443 --token dbl9yc.jmg4keymd7fage9b \
        --discovery-token-ca-cert-hash sha256:c32c1547c0f714dd2aad2732001d5b1409efce4bde85a5a8f29a7806e2136a6d



kubeadm join 192.168.1.40:6443 --token 2qatms.3c2vdr215kzq7asu \
        --discovery-token-ca-cert-hash sha256:19ccf2654e1e0e429c93d8cfb45a2d338c91611911e264c4950c2a74faf16c20

kubeadm join 192.168.100.160:6443 --token i4rxad.t9lrgc7k83wayrx4 \
        --discovery-token-ca-cert-hash sha256:66f0049f544c282d32042497d9abfc9cf1b4cd296d78a7fa6b85b1a31bdcc929


######	CRI-O	######


export OS="xUbuntu_22.04"
export VERSION="1.27"


echo "deb https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/ /" > /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list
echo "deb http://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable:/cri-o:/$VERSION/$OS/ /" > /etc/apt/sources.list.d/devel:kubic:libcontainers:stable:cri-o:$VERSION.list

curl -L https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable:/cri-o:/$VERSION/$OS/Release.key | apt-key add -
curl -L https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/Release.key | apt-key add -

apt-get update
apt-get install -y cri-o cri-o-runc


######		Kubernetes		######

These steps have to be performed on both nodes.

modprobe br_netfilter
echo 1 > /proc/sys/net/bridge/bridge-nf-call-iptables
echo 1 > /proc/sys/net/ipv4/ip_forward

$  modprobe br_netfilter

set net.bridge.bridge-nf-call-iptables to 1:

cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF

sudo sysctl --system

The container runtime has already been installed on both nodes, so you may skip this step.
Install kubeadm, kubectl and kubelet on all nodes:

sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl



curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.27/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.27/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt-get update

# To see the new version labels

sudo apt-cache madison kubeadm

swapoff -a


sudo apt-get install -y kubelet kubeadm kubectl

sudo apt-mark hold kubelet kubeadm kubectl


IP_ADDR=$(ip addr show ens160 | grep -oP '(?<=inet\s)\d+(\.\d+){3}')

kubeadm init --apiserver-cert-extra-sans=controlplane --apiserver-advertise-address $IP_ADDR --ignore-preflight-errors=all --pod-network-cidr=10.244.0.0/16
kubeadm init --apiserver-cert-extra-sans=controlplane --apiserver-advertise-address 192.168.100.160 --pod-network-cidr=10.244.0.0/16


Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 192.23.108.8:6443 --token udz7gw.ock7lve14hw0cdpp \
        --discovery-token-ca-cert-hash sha256:e7755ede5d6f0bae08bca4e13ccce8923995245ca68bba5f7ccc53c9b9728cdb 

root@controlplane:~> mkdir -p $HOME/.kube
root@controlplane:~> sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
root@controlplane:~> sudo chown $(id -u):$(id -g) $HOME/.kube/config

 kubeadm token create --print-join-command

kubeadm join 192.23.108.8:6443 --token thubfg.2zq20f5ttooz27op --discovery-token-ca-cert-hash sha256:e7755ede5d6f0bae08bca4e13ccce8923995245ca68bba5f7ccc53c9b9728cdb



root@node01:~> kubeadm join 192.23.108.8:6443 --token thubfg.2zq20f5ttooz27op --discovery-token-ca-cert-hash sha256:e7755ede5d6f0bae08bca4e13ccce8923995245ca68bba5f7ccc53c9b9728cdb 
[preflight] Running pre-flight checks
[preflight] Reading configuration from the cluster...
[preflight] FYI: You can look at this config file with 'kubectl -n kube-system get cm kubeadm-config -o yaml'
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Starting the kubelet
[kubelet-start] Waiting for the kubelet to perform the TLS Bootstrap...

This node has joined the cluster:
* Certificate signing request was sent to apiserver and a response was received.
* The Kubelet was informed of the new secure connection details.

Run 'kubectl get nodes' on the control-plane to see this node join the cluster.

curl -LO https://raw.githubusercontent.com/flannel-io/flannel/v0.20.2/Documentation/kube-flannel.yml


  args:
  - --ip-masq
  - --kube-subnet-mgr


kubectl apply -f kube-flannel.yml



curl -L https://download.opensuse.org/repositories/devel:kubic:libcontainers:stable:cri-o:$VERSION/$OS/Release.key | sudo apt-key --keyring /etc/apt/trusted.gpg.d/libcontainers.gpg add -
curl -L https://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/$OS/Release.key | sudo apt-key --keyring /etc/apt/trusted.gpg.d/libcontainers.gpg add -

