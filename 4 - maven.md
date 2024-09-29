Maven-Integration

1. Create an AWS instance for dev-server with regular method
    - Install git: *yum install git -y* 
    - Create ssh-key: *ssh-keygen*
    - Copy public ssh-key and add to github account: 
        - *cd /.ssh*
        - *cat id_rsa.pub*
    - Clone java application repo in home: *git clone git@github.com:Riddhi05s/new-proj.git*
    - Create new Repo in github account
    - Remove remote from local Repo: *git remote remove origin*
    - Add remote to new remote Repo: *git remote add orgin <repo_link>*
    - Push code to the new Repo: *git push origin main*

2. Create an AWS instance for Jenkins Server (choose medium, 15GB ram, Add custom TCP rule, port:8080, access anywhere)
    - Install git: *yum install git -y*
    - Install java: *yum install java-17-amazon-corretto -y*
    - Check java version: *java --version*
    - Add jenkins Repo: *sudo wget -O /etc/yum.repos.d/jenkins.repo \
    
      https://pkg.jenkins.io/redhat-stable/jenkins.repo*
    - Import key from jenkins CI: *sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key*
    - Install jenkins: *yum install jenkins -y*
    - Enable and start jenkins service: 
        - *sudo systemctl enable jenkins*
        - *sudo systemctl start jenkins*
    - Access jenkins dashboard on browser: *http://your_amazon_linux_instance_ip:8080*
    - Complete the steps in jenkins, access password using cat command

3. Establish connection between github repo and jenkins dashboard
    - Configure>> Genrate token: github_webhook
    - In github repo>> repo settings>> add webhook>> Copy path of jenkins http link>> Application/json>> Add token as security key>> check ifit gets green tick

4. Installing and configuring Maven
    - Install git on jenkins server: *yum install maven*
    - Check maven version: *mvn --version*
    - On jenkins dashboard install plugin **Maven Integration**
    - In installed plugins >> search github >> disable second >> enable third
    - In tools >> add java jdk path and maven path >> restart jenkins

5. Create new item for building project
    - select maven project
    - Add git repo in source code
    - change branch to main
    - save and apply
    - build 

