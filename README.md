# vagrant-k8s-env

## Start from scratch

- vagrant up

## Package a running vagrant k8s env for sharing

Sometimes, we want to reuse exsiting k8s running env, rather than building the env from scratch.

#### Package an exsiting k8s running env 

Firstly package the running env into a vagrant box:

- vagrant package --base kubeadm-v1.11 --output kubeadm-v1.11.box

#### Sharing

Then you can distribute this box for sharing. For example on another machine:

- install virtualbox (see [virtualbox.md](virtualbox.md)) and [vagrant](vagrant.md)
- mkdir ~/vagrant-k8s-env && cd ~/vagrant-k8s-env
- copy kubeadm-v.11.box to ~/vagrant-k8s-env
- vagrant box add kubeadm-v1.11.box --name kubeadm-v1.11
- prepare a `Vagrantfile` with following content:
    ```ruby
    Vagrant.configure("2") do |config|
      config.vm.box = "kubeadm-v1.11"
      config.vm.provider :virtualbox do |vb|
        vb.name = "istio-workshop"
      end
      config.ssh.insert_key = false
      config.ssh.username = "vagrant"
      config.ssh.password = "vagrant"
      config.vm.network "private_network", ip: "172.28.128.13"
    end
    ```
- vagrant up

#### Access

Up till now you should be to access the cluster either from inside the cluster, or on the host machine:

- from inside
    - vagrant ssh
    - kubectl get node
- from outside, on the host machine
    - mkdir ~/.kube
    - scp -P 2222 vagrant@127.0.0.1:/home/vagrant/.kube/config ~/.kube/ (password is "vagrant")
    - optional: if kubectl is not installed:
        - curl -L https://storage.googleapis.com/kubernetes-release/release/v1.11.3/bin/linux/amd64/kubectl -o /usr/local/bin/kubectl
        - chmod +x /usr/local/bin/kubectl
    - kubectl get node

---

## Misc useful commnds

- systemctl mask dev-dm-1.swap
