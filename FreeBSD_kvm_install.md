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
Find port and IP
```
virsh dumpxml freebsd | grep vnc
//<graphics type='vnc' port='5901' autoport='yes' listen='127.0.0.1'>
```
# 1. SSH
source: https://www.blackhole-networks.com/Cheatsheets/SerialConsole/

In FreeBSD VM:

Configure the bootloader to use the serial console by adding the line console="comconsole" to the /boot/loader.conf file.
```
echo 'console="comconsole"' >> /boot/loader.conf
```
Edit /etc/ttys and find the ttyu0 entry.

Serial terminals

The 'dialup' keyword identifies dialin lines to login, fingerd etc.
```
ttyu0	"/usr/libexec/getty std.9600"	dialup	off secure
```
Change this to:

Serial terminals

The 'dialup' keyword identifies dialin lines to login, fingerd etc.
```
ttyu0	"/usr/libexec/getty std.9600"	vt100	on secure
```
reboot

Try to connect on host:
```
virsh list --all
virsh connect <ID>
```

# 2. VNC Set Up SSH Tunneling on Linux
Open port 22 if needed on host, start ssh service
By default VNC connections don’t use encryption, so it is recommended to use an SSH Tunnel to secure your session.
```
sudo apt-get install openssh-server
sudo service ssh status    
sudo service ssh start
//(local ip found with "ip a"), test it, should connect, before port 22 closed error
ssh {user}@{KVM-host-IP-here} -L 5901:127.0.0.1:5901
```
example: ssh vivek@192.168.2.15 -L 5901:127.0.0.1:5901

`127.0.0.1`=localhost
        

VNC connection
```
sudo apt install tightvncserver
sudo apt-get install xvnc4viewer
xvncviewer localhost:5901
```
