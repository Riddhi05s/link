**Mount blob on azure virtual machine and ec2 instance**
Create resource group
Create storage account
Create container
Create virtual machine
Login to the virtual machine via terminal

Install all the necessary packages
```yml
cat /etc/*-release
wget https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb
dpkg -i packages-microsoft-prod.deb
apt-get update
apt-get install blobfuse
apt-get install libfuse2
```
Create temporary and permanent mount points and mount the container
```yml
mkdir /mnt/resource/blobfusetmp -p
mkdir ~/mycontainer
vim fuse-connection.cfg
chmod 600 fuse-connection.cfg
blobfuse ~/mycontainer --tmp-path=/mnt/resource/blobfusetmp  --config-file=fuse-connection.cfg -o attr_timeout=240 -o entry_timeout=240 -o negative_timeout=120
df -h
cd mycontainer/
touch mansi.txt{1..5}
ls
```
Check if the files are visible in the storage account container of the azure portal




