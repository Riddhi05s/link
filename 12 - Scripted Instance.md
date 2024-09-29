Steps:
1. Create instance by adding security HTTP rule with port 80
2. While creating instance in advance tab, add lines:
```bash
#!/bin/bash
yum install httpd -y
echo "This are the lines of code Adinath added in automated instance with script" >> /var/www/html/index.html
systemd start httpd
systemd enable httpd
useradd adinath -p redhat -c "Adinath Mahangare"
```
3. Launch instance
4. Copy public ip-addr and paste in another tab >> web page with above lines should open automatically

This will give assurance about the script you added while creating instance is working successfully.
