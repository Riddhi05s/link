1. Create VPC :10.0.0.0/16 (*mumbai-vpc*)
2. Create internet gateway and attach to VPC (*public-gtw*)
3. Create Subnets in different zones of region
   - public :10.0.0.0/24  (*public-subnet*)
   - private :10.0.1.0/24  (*private-subnet*)

4. Create Route Table for public subnet
   - Edit Routes :0.0.0.0 >> Add Internet Gateway >> *public-gtw*
   - Edit subnet association >> add public subnet >> save association

5. Create instance for web-server (public)
   - select correct VPC
   - select public-subnet
   - public-ip :enabled
   - add http rule in sg-group :0.0.0.0/0
   - create key-pair
   - try to connect instance to local device via ssh
   - host web-site
       - yum install httpd -y
       - cd /var/www/html
       - cat>index.html

         This is the content of html file

       - systemctl enable httpd
       - systemctl start httpd
  - try to access website through public-ip addr of web server instance

6. Create private db-server
   - select private subnet
   - access ip :disable

7. Create NAT gateway for connecting public-private servers
  - select public
  - connectivity type public
  - allocate Elastic IP

8. Important tip: Add route of each other after perring in outer-rt like 10.0.0.0/16
9. Use scp index.html root@10.0.1.178:/tmp
    
