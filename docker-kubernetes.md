DOCKER AND JENKINS
Host EKS Cluster
1. Create an instance for creating eks-cluster with Ubuntu as an Operating System and Allow All traffic. 
2. Create IAM users, allow below permissions to the users:
   AdministratorAccess, AmazonEC2FullAccess, AmazonRoute53FullAccess, AmazonS3FullAccess, IAMFullAccess
3. Create Roles, allow below permissions:
  AmazonEKSClusterPolicy,AmazonElasticContainerRegistryPublicFullAccess,IAMFullAccess
4. Launch instance in terminal :
Install aws cli and configure aws
```yml
apt-get update -y
apt install unzip -y
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
aws configure
```
Install EKS Tool
```yml
curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version
```
Install Kubectl
```yml
curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl 
kubectl version --client
```
Create EKS Cluster
```yml
eksctl create cluster --name my-cluster --region region-code --version 1.29 --vpc-public-subnets subnet-ExampleID1,subnet-ExampleID2 --without-nodegroup
```

5.	Create another instance ‘docker-server’ to create a docker image with amazon linux os
```yml
yum install docker -y
systemctl start docker
systemctl enable docker
yum install git -y
git clone https://github.com/abdulkadir18apr/Pallindrome-BirthDay.git
cd Palindrome-Birthday
ls
cd Pallindrome-BirthDay/
vim Dockerfile
```

paste the contents in the above file
```yml
# Use the official httpd image from the Docker Hub
FROM httpd:latest
 
# Copy the contents of the repository to the Apache document root
COPY . /usr/local/apache2/htdocs/
 
# Expose port 80
EXPOSE 80
 
# Start the Apache server
CMD ["httpd-foreground"]
```
Building docker image
```yml
docker build -t pallindrome-birthday .
#Running on localhost
docker run -d -p 80:80 pallindrome-birthday
docker images
aws configure
ssh-keygen
```

6. Go to aws.com > private registry > create repository > add name > create
Note: image name and repository should be same ‘pallindrome-birthday’ 

Enter the registry > view push commands > Run 1,3,4 commands as follows:
```yml
aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin 122610503177.dkr.ecr.ap-southeast-1.amazonaws.com
docker tag pallindrome-birthday:latest 122610503177.dkr.ecr.ap-southeast-1.amazonaws.com/pallindrome-birthday:latest
docker push 122610503177.dkr.ecr.ap-southeast-1.amazonaws.com/pallindrome-birthday:latest
```
7. Create a node-group:
```yml
eksctl create nodegroup \
  --cluster my-cluster \
  --region us-east-2 \
  --name my-node-group \
  --node-ami-family Ubuntu2004 \
  --node-type t2.small \
  --subnet-ids subnet-086ced1a84c94a342,subnet-01695faa5e0e61d97 \
  --nodes 3 \
  --nodes-min 2 \
  --nodes-max 4 \
  --ssh-access \
  --ssh-public-key /root/.ssh/id_rsa.pub
```
Check if your nodes are ready
```yml
kubectl get nodes 

mkdir /files
cd files
vim deployment.yaml
```
```yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: <your-eks-repo-url>/my-image:latest
        ports:
        - containerPort: 80
```
```yml
kubectl apply -f deployment.yaml
kubectl get deployment
kubectl get pods # Check if all are running
vim services.yaml
```
```yml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer
```
```yml
kubectl apply -f services.yaml
kubectl expose deployment/my-deployment
kubectl get svc -A
```
copy the external ip of my-deployment {a........amazon.com}

Run the copied ip in crome browser.
The webpage should be live.
