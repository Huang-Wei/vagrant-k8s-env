# -*- mode: ruby -*-
# vi: set ft=ruby :

# This script to install Kubernetes will get executed after we have provisioned the box 
$script = <<-SCRIPT
# Install kubernetes
apt-get update && apt-get install -y apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF
apt-get update
apt-get install -y kubelet kubeadm kubectl
# kubelet requires swap off
swapoff -a
# keep swap off after reboot
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab

# Get the IP address that VirtualBox has given this VM
# IFACE=$(ip r | grep default | awk '{print $NF}')
IFACE=eth1
IPADDR=$(ifconfig $IFACE | grep Mask | awk '{print $2}'| cut -f2 -d:)

# Set up Kubernetes
kubeadm init --apiserver-advertise-address=$IPADDR --pod-network-cidr=192.168.0.0/16
# Set up admin creds for the vagrant user
echo Copying credentials to /home/vagrant...
sudo --user=vagrant mkdir -p /home/vagrant/.kube
cp -i /etc/kubernetes/admin.conf /home/vagrant/.kube/config
chown $(id -u vagrant):$(id -g vagrant) /home/vagrant/.kube/config

# kubectl taint nodes --all node-role.kubernetes.io/master-
# kubectl apply -f https://docs.projectcalico.org/v3.1/getting-started/kubernetes/installation/hosted/rbac-kdd.yaml
# kubectl apply -f https://docs.projectcalico.org/v3.1/getting-started/kubernetes/installation/hosted/kubernetes-datastore/calico-networking/1.7/calico.yaml

echo "This VM has IP address $IPADDR"
echo "copy kubeconfig outside to host machine:"
echo "scp -P 2222 vagrant@127.0.0.1:/home/vagrant/.kube/config /tmp/kubeconfig"

SCRIPT

Vagrant.configure("2") do |config|
  config.vm.provider :virtualbox do |vb|
    vb.name = "kubeadm-v1.11"
  end
  # Specify your hostname if you like
  config.vm.hostname = "istio-workshop"
  config.ssh.password = "vagrant"
  config.vm.box = "bento/ubuntu-16.04"
  config.vm.network "private_network", type: "dhcp"
  config.vm.provision "docker"
  config.vm.provision "shell", inline: $script
end
