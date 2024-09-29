### *create EC2 instance with ansible installation*

- pip install boto3
- IAM user with AmazonEC2FullAccess, copy security key and access key
- in /etc/ansible/hosts add

```cfg
[local]
localhost ansible_connection=local
```

- aws configure
- create ansible-playbook with code

```yml
- name: Create EC2 instance
  hosts: local
  gather_facts: False
  tasks:
    - name: Launch EC2 instance
      amazon.aws.ec2_instance:
        key_name: ansible-kp
        instance_type: t2.micro
        image_id: ami-08718895af4dfa033
        region: ap-south-1
        wait: yes
        count: 1
        tags:
          Name: Ansible_EC2
      register: ec2
```

- run ansible-playbook: ansible-playbook aws-configure.yml
