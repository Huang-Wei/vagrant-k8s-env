## Install VirtualBox

#### Mac

#### Windows

#### Linux

Take Ubuntu 16.04 for example:

- download the `.deb` tarball: wget https://download.virtualbox.org/virtualbox/5.2.18/virtualbox-5.2_5.2.18-124319~Ubuntu~xenial_amd64.deb.
- install it: `sudo dpkg -i virtualbox-*_amd64.deb`
- verify it's installed properly: `VBoxManage --version`
- if some errors show up saying `The vboxdrv kernel module is not loaded`, following steps might help:
    - sudo apt-get update
    - sudo apt-get install -f dkms build-essential linux-headers-`uname -r`
    - sudo apt-get install -f
    - sudo apt-get install -f gcc make
    - sudo /sbin/vboxconfig
- reverify VirtualBox is installed properly: `VBoxManage --version`