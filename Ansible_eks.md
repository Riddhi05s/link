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
6. Generate ssh-key : ssh-keygen
7. Copy ssh-key to all managed-nodes (after changing node's sshd_config settings) : ssh-copy-id root@your_server_ip
8. Check all connections by ping : ansible all -m ping
9. Install pip on kubernetes server and then install openshift kubernetes : pip install openshift kubernetes
10. Create a directory on ansible server and create one ansible-playbook

```yml
---
- hosts: 172.31.5.42
  gather_facts: no
  tasks:
    - name: Deploy application in Kubernetes
      k8s:
        state: present
        definition:
          apiVersion: apps/v1
          kind: Deployment
          metadata:
            name: my-app
            namespace: default
            labels:
              app: my-app
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
                - name: my-app
                  image: 039612874025.dkr.ecr.ap-south-1.amazonaws.com/my-data:latest
                  imagePullPolicy: Always
                  ports:
                  - containerPort: 8080
            strategy:
              type: RollingUpdate
              rollingUpdate:
                maxSurge: 1
                maxUnavailable: 1
    - name: Copy service file to target
      copy:
        src: service.yml
        dest: /tmp/service.yml
 
    - name: Create Kubernetes Service
      command: kubectl apply -f /tmp/service.yml
      register: service_output
```

11. Create service.yml file in the same directory
```yml
apiVersion: v1
kind: Service
metadata:
  name: regapp-service
  labels:
    app: my-app
spec:
  selector:
    app: my-app

  ports:
    - port: 8080
      targetPort: 8080

  type: LoadBalancer
```
12. manually run ansible-playbook and check if service is created in kubernetes server, and get external ip of service to host application.


------------------------------------------------------------------------------------------------------------------------------------------------


### *after pushing image to ecr*

1. Create one emply ecs cluster
2. Task definitions >> Create new task definition
3. give some Task Definition Family
4. Give some container name and copy image URL
5. Create Task Definition
6. In previously created cluster create new service
7. Give service name
8. In netwroking section of service add 8080 TCP rule
9. Create service
10. One task will be automatically created
11. Check if its state in running
12. Copy ip address and use 8080 port with /webapp path
13. Successfully hosted image on container of ECS.
