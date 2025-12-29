## Terraform Modules

Modules are reusable, self-contained packages of terraform configurations that group together infrastructure resources for specfic purpose. 

### Why use modules?
- DRY (Dont't Repeat Yourself) --> no copy-pasting resources.
- Centralized fixes --> fix once, apply everywhere.
- Consistent security --> same tags, IAM, rules everywhere.

### Basic Module Structure?
    
    infra-modules/
    └── modules/
        └── S3/
            ├── main.tf
            ├── variables.tf
            ├── outputs.tf
            └── versions.tf


- Industry standard, not enforced.

### Minimal Example (S3 Module)

**1. main.tf**

        resource "aws_s3_bucket" "mybucket" {
            bucket = var.bucket_name
            force_destroy = var.force_destroy
        }

**2. variables.tf**

        variable "bucket_name" {
            type = string
        }

        variable "force_destroy" {
            type = bool
            default = false
        }

**3. outputs.tf**

        output "bucket_name" {
            description = "Name of the bucket."
            value = aws_s3_bucket.mybucket.id
        }

**4. using the Module**

        module "infra-mofules" {
            source = "./modules/s3"
            bucket_name = "kenish-ko-bucket"
            force_destroy = true
        }

### Provider Behaviour

- Modules do not create providers.
- Providers are inherited from root.
- For multiple regions, use alias

    provier "aws" {
        region = "us-east-1"
    }

    provider "aws" {
        alias = "eu"
        region = "eu-west-1"
    }

    module "eu_bucket" {
        source = ./modules/S3
        providers = { aws.eu }
    }

### Making Modules Reusable
A good reusable module must have:
- Clear variable names
- Sensible defaults
- Input validation
- Proper outputs

---