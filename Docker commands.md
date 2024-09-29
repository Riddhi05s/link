### Docker Commands

- rpmquery docker
- yum install docker -y
- docker info
- systemctl start docker
- systemctl enable docker
- docker info

- docker images
- docker pull httpd
- docker pull ubuntu
- docker ps 
- docker ps -a

- docker run -it --name webapp-01 ubuntu:latest /bin/bash
- exit
- docker inspect webapp-01 | less
- docker ps -a
- docker start webapp-01 
- docker attach webapp-01 


### inside webapp-01 
- apt update -y
- apt install apache2 -y
- service apache2 start 
- service apache2 status
- cd /var/www/html
- touch index.html
- cat > index.html 
  Added some lines from webapp-01
- cat index.html
- cd
ctrl + p  + ctrl + q    Outside of webapp-01
- curl 172.17.0.2:80    Nothing visible, since we haven't added port 80, so add port 80 in security rules, and try again
- docker ps -a
- curl 172.17.0.2

- docker run -it --name webapp-02 -p 8080:80 ubuntu:latest /bin/bash
- do the same procedure as done in previously, Website will be available at <public_ip>:8080, if you added tcp port 8080 in inbound security rules
- docker kill webapp-02     //all the data will be deleted, since it was stored on temporary storage


### // to add persistant volume to the container
- docker volume ls
- docker volume create myvol-01    //created a volume, stored at /var/lib/docker/volumes/myvol-01
- docker run -it --name webapp-03 -p 2222:80 -v my-vol:/var/www/html ubuntu:latest /bin/bash        //mounted volume on location /var/www/html
- add index.html, it will be available even if we kill th econtainer 
- docker kill webapp-03
- cd /var/lib/docker/volumes/myvol-01/_data 
- ls (index.html still visible)


- docker commit webapp-01 nginx-image:latest
- docker tag <image_id> ngnix:image:latest
