1. Create s3 bucket >> unique-name >> ACL enabled >> uncheck block all public access >> check I acknowledge >> create bucket
2. Add some file in bucket
3. Create access-key in IAM >> copy accesskey: secret-key
4. Create one EC2 instance with default settings
5. To connect instance with S3 bucket

   - yum install automake fuse fuse-devel gcc-c++ git libcurl-devel libxml2-devel make openssl-devel
   - git clone https://github.com/s3fs-fuse/s3fs-fuse.git
   - cd s3fs-fuse
   - ./autogen.sh
   - ./configure --prefix=/usr --with-openssl
   - make
   - sudo make install
   - which s3fs
   - touch /etc/passwd-s3fs
   - vim /etc/passwd-s3fs
   - copy accesskey:secretkey >> save
   - sudo chmod 640 /etc/passwd-s3fs
   - mkdir /mys3bucket
   - s3fs your_bucketname -o use_cache=/tmp -o allow_other -o uid=1001 -o mp_umask=002 -o multireq_max=5 /mys3bucket
   - df -Th
   - cd /mys3bucket
   - ll
   - touch index.html
   - check in s3 bucket, file will be visible
6. To view file, change permissions
