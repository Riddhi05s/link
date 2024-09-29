// use commands like *df -h*, *blkid*, *lsblk* whenever necessary

- created some data in original instance in folder /hdata
- created a volume
- attatched with instance
- assigned filesystem to volume :*mkfs.ext4 /dev/xvdb*
- create empty directory /bdata
- mounted xvdb on /bdata :*mount /dev/xvdb /bdata*
- or permanently mount by modifying fstab file: *vim /etc/fstab* (don't forget to restart daemons after this step: *systemd reload-daemons*, and *mount -a*)
- move data from /hdata to /bdata : __mv /hdata/* /bdata__ 

### Increasing size of external volume
- select volume>>actions>>modify volume>>select new volume>>modify
- now extended volume size is increased by we cannot access it, because it don't have any file-system
- but we cannot use mkfs to assign file-system, it will format whole extended volume
- so we will use: *resize2fs /dev/xvdb*   (resize2fs is only applicable for ext4 and not for xfs storage)
- now you can successfully use full volume of xvdb: *df -h*

### Decreasing size of external volume
- AWS will not allow you directly decrease size volume, so we have to create another volume of required size, and move data to that volume, and them remove previous volume
- Create new instance with desired size and attach it with our instance
- create new directory :*mkdir /cdata*
- mount xvdc on /cdata temporarily and permanently(in /etc/fstab): *mount /dev/xvdc /cdata*
- move data from bdata to cdata :__mv /bdata/* /cdata__
- unmount the xvdc: *umount /dev/xvdc* (or unmount permanently by commenting out the line from /etc/fstab)
- dettach the volume, and delete the volume
- *df -h* :to display result of decrease in size of extended volume

### Increasing size of root disk
- Modify size of volume from aws volumes, increase it to desired size
- Use command to get increased size for use: *xfs_growfs -d /dev/xvda1*
- *df -h* :to view resulted increase in size of root disk
  
