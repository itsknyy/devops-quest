**You can find the YAML CloudFormation (CFT) version here:**  
[View CFT YAML](https://github.com/itsknyy/devops-quest/blob/main/week-3/day-3/secure-s3-bucket.yaml)

---

# Secure AWS S3 Bucket Terraform Configuration

## 1. Creating the S3 Bucket

    resource "aws_s3_bucket" "secure_bucket" {
        bucket = "kenish-tf-secure-bucket"

        tags = {
        Name = "tf-s3"
        }
    }

- Creates an S3 bucket named kenish-tf-secure-bucket with a simple tag.

---

## 2. Enabling Versioning

    resource "aws_s3_bucket_versioning" "secure_bucket_versioning" {
        bucket = aws_s3_bucket.secure_bucket.id
    
        versioning_configuration {
        status = "Enabled"
        }
    }

- Enables object versioning to keep track of changes and deleted files (important for backup & recovery).

---

## 3. Ownership Controls

    resource "aws_s3_bucket_ownership_controls" "secure_bucket_ownership" {
        bucket = aws_s3_bucket.secure_bucket.id

        rule {
        object_ownership = "BucketOwnerPreferred"
        }
    }

- Ensures the bucket owner automatically owns uploaded objects, preventing permission issues.

---

## 4. Blocking Public Access

    resource "aws_s3_bucket_public_access_block" "secure_bucket_pab" {
        bucket = aws_s3_bucket.secure_bucket.id

        block_public_acls = true
        block_public_policy = true
        ignore_public_acls = true
        restrict_public_buckets = true
    }

- Applies all AWS recommended protections to ensure the bucket cannot become public accidentally.

---

## 5. Secure Bucket Policy

    resource "aws_s3_bucket_policy" "secure_bucket_policy" {
        bucket = aws_s3_bucket.secure_bucket.id


        policy = <<POLICY
        ...
        POLICY
    }

---

## 6. Default Server-Side Encryption (SSE)

    resource "aws_s3_bucket_server_side_encryption_configuration" "secure_bucket_encryption" {
        bucket = aws_s3_bucket.secure_bucket.id

        rule {
        apply_server_side_encryption_by_default {
        sse_algorithm = "AES256"
            }
        }
    }

- Automatically encrypts all objects using AES-256 if the uploader doesn’t specify encryption.

---