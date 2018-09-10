# vagrant-k8s-env

## Start from scratch

- choose a Vagrantfile to start with
    - ln -s {kubeadm-v.11|dind-v1.11}.Vagrantfile Vagrantfile
- vagrant up

## Package a running vagrant k8s env for sharing

Sometimes, we want to reuse exsiting k8s running env, rather than building the env from scratch.


## Package an exsiting k8s running env 

Firstly package the running env into a vagrant box:

- vagrant package --base kubeadm-v1.11 --output kubeadm-v1.11.box

## Sharing

Then you can distribute this box for sharing. For example on another machine:

- install virtualbox (see [virtualbox.md](virtualbox.md)) and vagrant
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
      config.ssh.password = "vagrant"
    end
    ```
- vagrant up

## Misc

- systemctl mask dev-dm-1.swap
