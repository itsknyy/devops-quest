# Terraform

## 1. Providers
Terraform cannot talk to cloud services directly. So it uses **providers**.
- Terraform downloads providers during terraform init.
- Provider handles:
    - Authentication
    - Translating Terraform Plans into API calls
    - Knowing what resources exist and how to manage them

---

## 2. Provider Requirements & Configurations
This tells Terraform which provider and version to use.

    terraform {
        required_providers {
            aws = {
            source  = "hashicorp/aws"
            version = "~> 5.0"
            }
        }
    }

Terraform uses this to download the correct provider.

### Configuration
This tells Terraform how to use the provider.

    provider "aws" {
        region = "ap-south-1"
    }
    provider "aws" {
        region = "us-east-1"
        alias  = "ue1"
    }

- Use aliases for multi-account or multi-region setups.
- Usage:  

        resource "aws_s3_bucket" "us_s3_bucket" {
            provider = aws.ue1
            bucket   = "my-us-s3-bucket-yay"
        }

---

## 3. Resources
A Resource is a Declarative Description.

    resource "aws_s3_bucket" "logs" {
        bucket = "my-logs-bucket"
    }

Terraform stores a resource using its address: `aws_s3_bucket.logs`

If the person renames the resource block, Terraform thinks:
- Old is deleted
- New is created  

Because the address changed.

---

## 4. Resource Lifecycle Symbols

|Symbol  |	Meaning                              |    
|------- |---------------------------------------|
|+	     | Create new resource                   |
|~	     | Modify/update the resource            |
|-	     | Delete resource                       |
|-/+     | Replace → delete old then create new  |

Example:
- Changing instance type = ~ (update)
- Changing the AMI = -/+ (replacement)

---

## 5. Data Sources
A Data Source lets Terraform read existing infrastructure. 
**Data sources never create anything.**

    data "aws_ami" "ubuntu" {
    most_recent = true
    }
Use it in a resource like:

    ami = data.aws_ami.ubuntu.id

- **Resource = create**
- **Data Source = read**

---

## 6. State Management
Terraform keeps a file called terraform.tfstate. This file remembers:
- What Terraform created
- IDs, ARNs, IPs
- Attributes of resources  

State is important because without it Terraform would:
- Recreate everything every time
- Couldn’t detect changes

**State is NOT the source of truth - the code is.**  
State is just Terraform’s **“memory”**.

---



