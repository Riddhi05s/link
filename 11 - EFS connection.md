- create three instances :amazon-linux, redhat, ubuntu
- add security group inbound rule: NFS >> port: 2049 >> Allow Everywhere
- connect all three instances to local device via ssh
- update all three instances to their newest version: *yum update -y*, *apt update -y*
- install nfs-utils package on all three instances
  - amazon linux: already installed
  - redhat: *yum install nfs-utils -y*
  - ubuntu: *apt install nfs-common -y*
 
- AWS >> EFS >> Create File System >> check unique ip for each zone of the region
- Add previously created security group via network >> manage
- in amazon linux instance create directory for mounting data: *mkdir /data*
- in aws EFS >> attach >> copy command for ip (check if zone matches with instance zone)
- paste command in instance terminal and mount to created directory
- add files to the mounted directory: *touch index.html{1..100}*
- go in redhat instance terminal >> create new directory >> mount directory with coping command >> check if data is visible

## Replication of file system
- create new Elastic File System on another region(ex.singapore)
- create new instance in that region >> create security group with NFS rule >> attatch same group to File system all zones
- Go in mumbai Elastic File System >> replication >> select destination >> select Singapore File System >> and replicate
- After completion of replication process, you will be able to access data via instance terminal
