### After successfully pushing image to ECR

1. Create one ansible server and one kubernetes server
2. Do kubectl and eksctl installation on kubernetes server and create cluster with nodegroup
3. Do ansible installation on ansible server : sudo apt install ansible -y
4. Create an inventory file : sudo vim /etc/ansible/hosts
5. If you are using another name for inventory file, and default command in ansible.cfg file as below:
```cfg
[defaults]
inventory = /etc/ansible/hosts
```
7. Add your managed nodes with private_ip_addr with below format
```cfg
[servers]
server1 ansible_host=your_server_ip
server2 ansible_host=your_server_ip
```
