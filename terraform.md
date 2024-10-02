### Terraform : Infrastructure as a code

Infrastructure as Code (IaC) is a method of managing and provisioning computing infrastructure through machine-readable definition files, rather than through physical hardware configuration or interactive configuration tools

- AWS Cloud : CloudFormation / CloudForm
- Azure : ARM Template
- GCP : Google Deployment
- Private Cloud (OpenStack) : Heat Template

## Save resources and Utilize resources efficiently

- Micheal Hashi : Founder of terraform
- Simple Plain Text : HCL(Hashikorf Configuration Language)
- Terraform, Kubernetes are developed in Golang, Jenkins developed in Java

### Three types of IaC 
1. Configuration Management Tool >> Configures application on the server > Ansible, Puppet, SaltStack, Chef
2. Server Templating Tool >> Make Template of running Server for backup > AWS cli, Docker, Packer, Vagrant
3. Infrastucture Provisioning Tool >> define and provision infrastructures accross various cloud providers using declarative language like yaml, hcl > Terraform, AWS CloudFormation, Azure Resource Manager (ARM), Google Cloud Deployment Manager

.tf >> template file

### different ways to connect terraform instance with aws
1. aws configure
2. aws provider

--------------------------------------------------------------------------------------------------------------
LAB01 : Creating EC2 instance using Terraform

1. Install terraform
	- sudo yum install -y yum-utils
	- sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo
	- sudo yum install terraform -y

2. Connect terraform server to cloud (aws) using AWS Provider of AWS configure
3. Initialize Terraform
4. create aws instance using terraform

```
provider "aws" {
  region  = "ap-south-1"
  access_key = "aws_access_key"
  secret_key = "aws_secret_key"
}

resource "aws_instance" "Dev_server" {
  ami           = "ami-08718895af4dfa033"
  instance_type = "t2.micro"
  key_name      = "trf-kp"
}
```
5. Validate Terraform
6. Plan Terraform
7. Apply Terraform

terraform init >> terraform validate >> terraform plan >> terraform apply

8. enable SSH in security group
9. Connect to the instance

--------------------------------------------------------------------
### Important commands in terraform:

1. terraform fmt : format code per HCL canonical standard
2. terraform validate : validate code for syntax
3. terraform init : initialize directory, pull down providers
4. terraform plan -out plan.out : use the plan.out file to deploy infrastructure
5. terraform apply --auto-approve : apply changes without being prompted to enter "yes"
6. terraform destroy : destroy/cleanup deployment
7. terraform state show aws_instance.Dev_server : show details stored in Terraform state for the resource

--------------------------------------------------------------------------------
LAB02 :  EBS attachment 
```
provider "aws" {
  region = "ap-south-1"
}

#security group
resource "aws_security_group" "webserver-sg" {
  name        = "webserver-sg"
  description = "allow ssh and http"

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

}

#create instance
resource "aws_instance" "EC2-instance-1" {
  ami               = "ami-08718895af4dfa033"
  availability_zone = "ap-south-1a"
  instance_type     = "t2.micro"
  security_groups   = ["${aws_security_group.webserver-sg.name}"]
  key_name          = "trf-kp"
  tags = {
    Name = "EC2-instance-1"
  }
}

#create block storage
resource "aws_ebs_volume" "data_vol" {
  availability_zone = "ap-south-1a"
  size              = 5
}

resource "aws_volume_attachment" "EC2-instance-1-vol" {
  device_name = "/dev/sdc"
  volume_id   = aws_ebs_volume.data_vol.id
  instance_id = aws_instance.EC2-instance-1.id
}

```
--------------------------------------------------------------------------------------
LAB03 : Creating vpc and attaching instance with subnet

```
#assigning aws provider
provider "aws" {
  region = "ap-south-1"
}

#creating vpc
resource "aws_vpc" "main" {
  cidr_block       = "10.0.0.0/16"
  instance_tenancy = "default"

  tags = {
    Name = "main"
  }
}

#creating internet gateway
resource "aws_internet_gateway" "gw" {
  vpc_id = aws_vpc.main.id

  tags = {
    Name = "gw"
  }
}

#creating subnet in main vpc
resource "aws_subnet" "public" {
  vpc_id            = aws_vpc.main.id
  cidr_block        = "10.0.0.0/24"
  availability_zone = "ap-south-1a"
  tags = {
    Name = "public"
  }
}

#creating route table
resource "aws_route_table" "public-rt" {
  vpc_id = aws_vpc.main.id

  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.gw.id
  }

  tags = {
    Name = "public-rt"
  }
}

#connecting route table with subnet
resource "aws_route_table_association" "a" {
  subnet_id      = aws_subnet.public.id
  route_table_id = aws_route_table.public-rt.id
}

#creating security group for ec2 instance
resource "aws_security_group" "vpc-sg" {
  name   = "newvpc"
  vpc_id = aws_vpc.main.id

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "vpc-sg"
  }
}

#creating elastic ip
#resource "aws_eip" "eip1" {
#  domain = "vpc"
#}

#creating aws instance
resource "aws_instance" "test" {
  ami                    = "ami-08718895af4dfa033"
  instance_type          = "t2.micro"
  subnet_id              = aws_subnet.public.id
  vpc_security_group_ids = [aws_security_group.vpc-sg.id]
  key_name               = "trf-kp"
  tags = {
    Name = "test"
  }
}

#creating eip association with instance
resource "aws_eip_association" "eip_assoc" {
  instance_id   = aws_instance.test.id
  allocation_id = "eipalloc-0cc9adb3c92ecc44b"
}
```
-----------------------------------------------------------------------------------------------
- sonarcube : security analysis of devops : devSecOps
LAB04:  Custom-size for root-volume for EC2 instance

```
provider "aws" {
  region = "ap-south-1"
}

resource "aws_security_group" "root-sg" {
  name        = "root-sg"
  description = "security group for modifying root volume of instance"

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}

resource "aws_instance" "test-volume" {
  ami               = "ami-08718895af4dfa033"
  availability_zone = "ap-south-1a"
  instance_type     = "t2.micro"
  security_groups   = [aws_security_group.root-sg.name]
  key_name          = "trf-kp"

  #root disk
  root_block_device {
    volume_size           = "26"
    volume_type           = "gp2"
    delete_on_termination = true
  }
}
```
--------------------------------------------------------------------------------------------------------------------------------







