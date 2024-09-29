1. Create new instance
2. Connect via ssh to local device
3. *lsblk*
4. Elastic book store>> Volume >> Create Volume
5. Select size 5 GB
6. Select same region as the instance
7. Select Volume >> Actions >> Attatch Volume
8. Select instance by instance-id
9. Select device like /dev/sdb
10. Go to instance >> *lsblk* :you'll see partitions with xvdb, but you can't access it because it don't have filesystem, and not mounted
11. *lsblk -fs* :will show partitions with filesystem, xvdb not available
12. *blkid* :will show all disks with their UUID and partUUID, xvdb not available, DB: data block
13. *mkfs* + tab + tab : display all different types of file systems
14. *mkfs.ext4 /dev/xvdb* :make file system ext4 on xvdb
15. *lsblk -fs* :svdb will be present in it
16. *blkid* :dev/xvdb present
17. *mkdir /data*
18. *mount /dev/svdb /data* :mount data directory on svdb    :temporary mounting, will be removed automatically after instance reboot
19. *df -h* :you can see /data folder mounted on svdb partition
20. *cat /data/training.txt* :add some data in training.txt

21. Reboot Instance
22. *lsblk*  :xvdb not connected
23. *df -h*  : no xvdb present
24. *blkid*  : copy uuid of xvdb
25. *vim /etc/fstab* :add below lines    :permanent mounting

UUID="2fcc4dc3-04d1-47a6-9dcf-e60fe892a172"  /data  ext4  defaults  0   0

26. mount -a

27. *df -h* :mounted xvdb
28. Reboot system
29. *df -h* :still can see as mounted
