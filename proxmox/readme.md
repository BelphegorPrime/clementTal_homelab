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
On LXC
```
groupadd -g 10000 lxc_shares
usermod -aG lxc_shares root
reboot
touch /FOLDER/foobar
rm /FOLDER/foobar
```
