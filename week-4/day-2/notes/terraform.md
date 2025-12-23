# Terraform Variables & Outputs

## 1. VARIABLES (Inputs)
- Named inputs that allow parameterization.
- Helps reuse values, make configs flexible.

**Example:**

    variable "region" {
    description = "AWS region"
    type        = string
    default     = "us-east-1"
    }

    provider "aws" {
    region = var.region
    }

---

## 1.1. Variables Types

- string
- number
- bool

**Example:**

    variable "enable_monitoring" {
        type    = bool
        default = true
    }

- list(type)
- map(type)

**Example:**

    variable "availability_zones" {
    type = list(string)
    }

    variable "tags" {
    type = map(string)
    }

**Usage:**  
**List**
`terraform.tfvars`

    availability_zones = ["us-east-1a", "us-east-1b"]

**Map**

    tags = {
    Environment = "dev"
    Owner       = "team-a"
    }

---

## 1.2. Variable Precedence (Lowest --> Highest priority)

1. default value (inside variable block)
2. terraform.tfvars
3. *.auto.tfvars
4.  -var CLI flag
5. -var-file

---

## 2. Input Validation

Used to restrict invalid inputs before apply.

**Example:**

    variable "environment" {
    type = string

    validation {
        condition     = contains(["dev", "staging", "prod"], var.environment)
        error_message = "Environment must be dev, staging, or prod."
    }
    }

Benefits:
- Fewer runtime errors
- Safer refactors
- Cleaner configuration

---

## 3. OUTPUTS (Results)

- Values printed after terraform apply.
- Stored in the state file.
- Useful for:
    - human visibility
    - cross-module communication
    - automation scripts

**Example Output:**

    output "instance_ip" {
        description = "Public IP"
        value       = aws_instance.web.public_ip
    }

**After apply:**

    instance_ip = "89.123.45.67"

--- 

## 3.1. Output Behavior

- Calculated after resources are created.
- Stored in state.
- Recomputed on every apply.
- Sensitive outputs can hide values:

    output "db_password" {
    value     = var.db_password
    sensitive = true
    }

(Masked in CLI but still in state!)

---

