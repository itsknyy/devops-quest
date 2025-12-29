## Task: Introduce Modules on your existing Terraform stack with Variables and Outputs. You might need to create a new branch from the previous branch (not source/main branch).

### 1. Project Structure

    EC2-S3-Terraform/
    │
    ├── main.tf                # Root Terraform configuration
    ├── variable.tf            # Input variables for the whole project
    ├── .gitignore
    │
    └── infrastructure-modules/
        ├── s3/                # Reusable S3 module
        │   ├── main.tf        # Defines aws_s3_bucket resource
        │   ├── variables.tf   # Variables: bucket_name.
        │   └── outputs.tf     # Outputs: bucket_name.
        │
        └── ec2/               # Reusable EC2 module
            ├── main.tf        # Defines aws_instance resource
            ├── variables.tf   # Variables: vpc_cidr, instance_type, etc.
            └── outputs.tf     # Outputs: VPC ID, instance_ip, instance_id.

