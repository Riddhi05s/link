1. Create a new redhat instance : base-ami
2. Add new HTTP port 80 in security groups
3. *yum update -y*

4. *yum install httpd -y*   :Hypertext Transfer Protocol daemon
5. *cd /var/www/html/*
6. *cat>index.html*
   This is Adinath's Page
7. *systemctl enable httpd*
8. *systemctl start httpd*
9. *ip a s*
10. *curl http://local_public_ip*
11. You will be able to see website hosted on instance public ip add of aws

12. Install some packages on base-ami
    - *yum install vsftpd -y*   :Very secure FTP daemon
    - *yum install cifs-utils -y*   :Common Internet File System
    - *yum install nfs-utils -y*   :Network File System
    - *yum install tree -y*   :Produces a depth-indented listing of files
13. *cd /tmp*
14. *touch index.html{1..100}*  :which will create 100 files named index.html

15. Now stop the instance
16. Create image from Actions, with same security group, key-pair, subnet.
17. Start the base instance
    
19. Create new instance with created custom ami
20. While connecting via ssh, change root to ec2user
21. Copy public ip addr and check if it is shwoing content in index.html
22. Connect to local device via ssh
23. Go in /tmp, and check if all files are available
24. Check package availability using rpmquery command, ex. *rpmquery vsftpd*

    Successfully used custom-ami
    
