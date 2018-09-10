# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.provider :virtualbox do |vb|
    vb.name = "dind-v1.11"
  end
  # Specify your hostname if you like
  # config.vm.hostname = "dind-v1.11"
  config.ssh.password = "vagrant"
  config.vm.box = "bento/ubuntu-16.04"
  config.vm.network "private_network", type: "dhcp"
  config.vm.provision "docker"
  config.vm.provision "shell" do |s|
    s.path = "https://cdn.rawgit.com/kubernetes-sigs/kubeadm-dind-cluster/master/fixed/dind-cluster-v1.11.sh"
    s.args = "up"
  end
end
