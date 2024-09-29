Build 3 instances t2.micro on amazon linux, controller, host-manager-1 and host-manager-2
-hostnamectl set-hostname controller
-bash
-ssh-keygen
ip a s
cd .ssh
vim authorized_keys
cat id_rsa.pub

Here incase of controller copy paste the public key of other two instances and paste the keys in the vim file and likewise do the same for other 2 instances to establish connection
Controller
systemctl start sshd
systemctl enable sshd
Use ping command to check if the connection is established between the instances and allow ICMP port in network security
Controller - Install ansible
yum install ansible* -y
ansible --version
cd /etc/ansible/
vim ansible.cfg
Paste the below content inside the vim file
