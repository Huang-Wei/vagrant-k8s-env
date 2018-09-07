# vagrant-k8s-env

## Start from scratch

- vagrant up

## Package a running vagrant k8s env for sharing

Sometimes, we want to save time building the vm from scratch. In order to achieve that, firstly package the running env to a vagrant box:

- vagrant package --base k8s-v1.11 --output k8s-v1.11.box

Then you can distribute this box for sharing. For example on another machine:

- install virtualbox (see [virtualbox.md](virtualbox.md))
- mdkir ~/vagrant-k8s-env && cd ~/vagrant-k8s-env
- copy k8s-v.11.box to ~/vagrant-k8s-env
- vagrant init -m k8s-v1.11
- vagrant up
