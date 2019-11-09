# Virsh install
```
sudo apt-get install qemu-kvm libvirt-clients libvirt-daemon-system virtinst
sudo virsh list --all
```

Find os name:
```
sudo apt-get install libosinfo-bin
osinfo-query os
```

# FreeBSD installation
Launch and follow the commands in Virt Viewer, use default value, create user
```
virt-install -n freebsd -r 512 --vcpus=1 --os-variant=freebsd12.0 --accelerate -v \
-c ./FreeBSD-12.1-RELEASE-amd64-disc1.iso --vnc --disk path=~/Documents/kvm/freebsd12.img,size=4
```

# Virsh abort install
```
virsh destroy _domain-id_
virsh undefine _domain-id_
virsh vol-delete _domain-id_.img
```

# Things to install after the installation
```
# pkg  
# pkg install sudo
```
add user with visudo (as root)

default shell is tcsh, to change
```
chsh -s /usr/local/bin/bash someuser
```
Vi mode in tcsh with `binkey -v`

# Launch & Usage
```
virsh list --all
virsh start freebsd
virt-viewer freebsd
virsh shutdown freebsd
```
# Network
```
sudo virsh net-start default 
virsh net-edit default
sudo virsh net-list --all
sudo virsh net-autostart --network default
```
