1. Prerequisites:
Multiple Linux servers (at least 1 for master and 1 or more for worker nodes).
A user with sudo privileges on these servers.
All nodes should have swap disabled, as Kubernetes recommends.

2. Install Docker:
On all nodes (master and workers):

```
sudo apt-get update
sudo apt-get install -y docker.io
```
3. Install Kubernetes Components:
On all nodes:

Add Kubernetes apt repository:

```
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
```

Install kubelet, kubeadm, and kubectl:

```
sudo apt-get install -y kubelet kubeadm kubectl
Hold them at the current version:


```

sudo apt-mark hold kubelet kubeadm kubectl

4. Setting up Master Node:
Initialize the master node:

```

sudo kubeadm init --pod-network-cidr=10.244.0.0/16
This CIDR is for Flannel networking; adjust if you're using another CNI.

After initialization, you'll see a kubeadm join command displayed. Save this command, as it will be used to join worker nodes.

Set up the local kubeconfig:
```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
Install a Pod network:
Using Flannel as an example:

```

kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
```

5. Setting up Worker Nodes:
On each worker node, run the kubeadm join command you saved earlier.

6. Verify the Cluster:
On the master node:

```

kubectl get nodes

```