**You can find the YAML CloudFormation (CFT) version here:**  
[View CFT YAML](https://github.com/itsknyy/devops-quest/blob/main/week-2/day-5/IAC/ec2.yaml)

---

# 1. Terraform Block

The terraform block tells Terraform which providers and versions to use.

    terraform {
        required_providers {
            aws = {
            source = "hashicorp/aws"
            version = "~>5.0"
            }
        }
    }

---

# 2. Provider Block

This tells Terraform which AWS region to deploy resources in.

    provider "aws" {
        region = "us-east-1"
    }

---

# 3. Data Source – AMI Lookup

Automatically fetch the latest Amazon Linux 2 AMI.

    data "aws_ami" "amazon_linux" {
        most_recent = true
        owners = ["amazon"]

        filter {
            name = "name"
            values = ["amzn2-ami-hvm-*-x86_64-gp2"]
        }
    }

---

# 4. VPC

    resource "aws_vpc" "main" {
        cidr_block = "10.0.0.0/16"
        enable_dns_support = true
        enable_dns_hostnames = true

    tags = {
        Name = "tf-VPC"
        }
    }

- enable_dns_support: Allows EC2 instances inside the VPC to resolve DNS names (e.g., convert domain → IP).
- enable_dns_hostnames: Allows EC2 instances to receive public DNS hostnames (e.g., ec2-54-23-12-10.compute.amazonaws.com).

---

# 5. Public Subnet

    resource "aws_subnet" "public" {
        vpc_id                    = aws_vpc.main.id
        cidr_block                = "10.0.0.0/24"
        availability_zone         = "us-east-1a"
        map_public_ip_on_launch   = true

    tags = {
    Name = "tf-Public-Subnet"
    }
    }

- map_public_ip_on_launch = Automatically assigns a public IP address to any EC2 instance launched in that subnet.

---

# 6. Internet Gateway (IGW)

    resource "aws_internet_gateway" "igw" {
        vpc_id = aws_vpc.main.id

        tags = {
        Name = "tf-IGW"
        }
    }

- IGW enables communication between VPC and the internet.

---

# 7. Route Table

    resource "aws_route_table" "public" {
        vpc_id = aws_vpc.main.id

    tags = {
        Name = "tf-Route-Table"
        }
    }

- A route table for the public subnet.

---

# 8. Route to Internet

    resource "aws_route" "public_inet" {
        route_table_id = aws_route_table.public.id
        destination_cidr_block = "0.0.0.0/0"
        gateway_id = aws_internet_gateway.igw.id
    }

- Sends all outbound traffic (0.0.0.0/0) to the Internet Gateway.

---

# 9. Subnet Route Table Association

    resource "aws_route_table_association" "public_assoc" {
        subnet_id = aws_subnet.public.id
        route_table_id = aws_route_table.public.id
    }

- Associates the public subnet with the public route table.
- This makes the subnet public.

---

# 10. Security Group

    resource "aws_security_group" "web" {
        name = "TF-Web-SG"
        description = "Allow SSH & HTTP"
        vpc_id = aws_vpc.main.id


    ingress {
        from_port = 22
        to_port = 22
        protocol = "tcp"
        cidr_blocks = ["0.0.0.0/0"]
    }


    ingress {
        from_port = 80
        to_port = 80
        protocol = "tcp"
        cidr_blocks = ["0.0.0.0/0"]
    }


    egress {
        from_port = 0
        to_port = 0
        protocol = "-1"
        cidr_blocks = ["0.0.0.0/0"]
        } 
    }

---

# 11. EC2 Instance

    resource "aws_instance" "web" {
        ami = data.aws_ami.amazon_linux.id
        instance_type = "t3.micro"
        subnet_id = aws_subnet.public.id
        key_name = "stack-key" # key should be changed
        vpc_security_group_ids = [aws_security_group.web.id]
    }

---

# 12. Elastic IP

    resource "aws_eip" "web_ip" {
        instance = aws_instance.web.id
        domain = "vpc"
    }

- Allocates a stable public static IP.
- Attached to the EC2 instance.

---



