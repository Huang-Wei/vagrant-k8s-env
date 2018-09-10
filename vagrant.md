## Install Vagrant

## Ubuntu

Take Ubuntu 16.04 for example:

- download the `.deb` tarball: wget https://releases.hashicorp.com/vagrant/2.1.4/vagrant_2.1.4_x86_64.deb
- install it: `sudo dpkg -i vagrant-*64.deb`
- (optionally): `sudo apt-get install -f` if previous failed
- verify it's installed properly: `vagrant version`