# TrueNas
## Disk passthroug
```
apt install lshw
lshw -class disk -class storage 
ls -l /dev/disk/by-id/ # get disk ID
qm set VM_ID -scsi5 /dev/disk/by-id/DISK_ID # replace VM_ID and DISK_ID
```

# Misc
## Share NFS through LXC
On Host:
```
mkdir /mnt/MOUNT/FOLDER
nano /etc/fstab
NFS_IP_ADDR:NFS_SHARE_TO_MOUNT /mnt/MOUNT/FOLDER nfs defaults 0 0
systemctl daemon-reload
mount -a

nano /etc/pve/lxc/LXC_ID.conf
mp0: /mnt/MOUNT/FOLDER/,mp=/FOLDER
```
On LXC (optional)
```
groupadd -g 10000 lxc_shares
usermod -aG lxc_shares root
reboot
touch /FOLDER/foobar
rm /FOLDER/foobar
```

# Firewall
[doc](https://rdr-it.com/proxmox-utiliser-le-firewall-integre-pour-securiser-les-machines-virtuelles/)

# Qbitorrent
Mix of: 
https://stinky.blog/blog/openvpn-inside-lxc-container/
and 
https://www.htpcguides.com/autoconnect-private-internet-access-vpn-boot-linux/

1. sur l'host
```
nano /etc/pve/lxc/120.conf
chown 100000:100000 /dev/net/tun
```

3. sur le LXC qbitorrent
```
lxc.cgroup2.devices.allow: c 10:200 rwm
lxc.mount.entry: /dev/net dev/net none bind,create=dir

apt install openvpn
sudo wget --no-check-certificate https://www.privateinternetaccess.com/openvpn/openvpn.zip
sudo unzip openvpn.zip
sudo nano /etc/openvpn/login.txt
```
```
login
password
```
```
sudo chmod 700 /etc/openvpn/login.txt
```

#test:
```
cd /etc/openvpn
sudo openvpn --config /etc/openvpn/Sweden.ovpn --auth-user-pass /etc/openvpn/login.txt
```

# service
```
nano /etc/systemd/system/openvpn@pia.service
```
```
[Unit]
Description=OpenVPN connection
After=network.target

[Service]
Type=simple
ExecStart=/usr/sbin/openvpn --config /etc/openvpn/switzerland.ovpn --auth-user-pass /etc/openvpn/login.txt
Restart=on-failure

[Install]
WantedBy=multi-user.target
```
```
systemctl daemon-reload
systemctl enable openvpn@pia
systemctl start openvpn@pia
```
3. Dans qbitorrent
Changer l'interface réseau par tun0

voila
