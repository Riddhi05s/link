### After successfully pushing image to ECR

Create one ansible server and one kubernetes server
Do kubectl and eksctl installation on kubernetes server and create cluster with nodegroup
Do ansible installation on ansible server : sudo apt install ansible -y
Create an inventory file : sudo vim /etc/ansible/hosts
If you are using another name for inventory file, and default command in ansible.cfg file as below:
---cfg
[defaults]
inventory = /etc/ansible/hosts
---
