### *Linux Basic Commands*

**Steps:**

### 1. Create AWS EC2 instance for amazon linux and connect with command prompt.
### 2. Basic commands for regular user
   - *ls*     list, -a: show hidden
   - *ll*     detailed list with permissions
   - *df*     display disk filesystem info
   - *whoami* shows current user name
   - *rm*     remove file
   - *pwd*    present working directory
   - *mkdir <directory_name>*  create directory
   - *rmdir <directory_name>*  remove directory, -r: recursively
   - *touch <file_name>* creates a file
   - *cat <file_name>* display contents in file, > add contents, >> appends contents
   - *date*   current date and timestamp
   - *cal*    calender
   - *cd <directory_name>*     change directory
   - *mv <old_name> <new_name>*     move file or rename
   - *cp <source_file> <destination_path>*  copy source file to destination
   - *exit* exits from current user
   - *clear* clears terminal
   - *history* shows command history
   - *wget <http_link>* direct download from internet


### 3. Basic commands for root user
   - *sudo su -*    switch user to root
   - *whereis <linux_command>* shows path where command is stored
   - *ps* display active processes, -a: all
   - *chmod <permission_number> <file_path>* changing permission for file/directory, owner-group-others
        - read: 4
        - write: 2
        - execute: 1
   - *chgrp <user_name> <destination_path>* changing group of file/directory
   - *useradd <user_name>* adding new user, users saved in /etc/passwd
   - *<user_name> passwd* adding password to user, passwords saved in /etc/shadow
   - *kill <process_id>* killing process by PID
   - *ssh-keygen* creating public-private key pair, stored at .ssh file in home directory, .pub: public key
   - *systemctl start/stop/enable/disable/status <service_name>* manages system serices states, example: sshd



### 4. Adding sudoers

- useradd virat
- passwd virat
- tail /etc/passwd
- whereis useradd   (copy first path)
- whereis passwd    (copy first path)
- vim /etc/sudoers
- add lines after root
  virat   ALL=(ALL)       /usr/sbin/useradd, /usr/bin/passwd, ! /usr/bin/passwd root

  (useraname)   (all hosts: all users)      (commands that can be run by sudoer)

    we are giving sudoer permission to add new user, change password, but he should not change password of root
- su virat      (switch user to virat)
- sudo useradd rohit    (add rohit to machine as sudoer)
- sudo passwd rohit     (add password for rohit as sudoer)

- sudo passwd root

  This message should displayed:  (Sorry, user virat is not allowed to execute '/usr/bin/passwd root' as root on practice.example.com.)
- exit 



### 5. Group commands

- groupadd devops   (add new group to machine)
- useradd rutik     (add user rutik to machine) 
- useradd snehal
- usermod -G devops rutik       (add user rutik to group devops)
- usermod -G devops snehal
- tail /etc/group
- grpadd cloud
- usermod -aG cloud rutik       (append user rutik to group cloud, linux will only allow one group per user)
- mkdir devops-practice
- ll


*output: drwxr-xr-x. 2 root root 6 Sep  2 03:49 devops-practice*
- chgrp devops devops-practice      (changing default group to devops)

*output: drwxr-xr-x. 2 root devops 6 Sep  2 03:49 devops-practice*

- chown rutik devops-practice       (changing default owner of resource)

*output: drwxr-xr-x. 2 rutik devops 6 Sep  2 03:49 devops-practice*

- chmod 740 devops-practice         (chaning permissions for resource)

*output: drwxr-----. 2 rutik devops 6 Sep  2 03:49 devops-practice*

- chmod o+t devops-practice         (adding sticky-note to resource), other users cannot delete this resource

*output: drwxr----T. 2 rutik devops 6 Sep  2 03:49 devops-practice*

- cat /etc/passwd | grep rutik  (finding information about perticular user)

*output: rutik:x:1003:1004::/home/rutik:/bin/bash* 



