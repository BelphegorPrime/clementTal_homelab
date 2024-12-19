# TrueNas
## Disk passthroug
```
apt install lshw
lshw -class disk -class storage 
ls -l /dev/disk/by-id/ # get disk ID
qm set VM_ID -scsi5 /dev/disk/by-id/DISK_ID # replace VM_ID and DISK_ID



