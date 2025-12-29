# Terraform Drifting

## 1. What is Drift?

- When real infrastructure doesn't match your Terraform code
- Happens when someone makes changes outside Terraform
- Creates mismatch between: code -> state file -> actual infrastructure

## 2. Why Drift Happens

- Manual changes in cloud console (AWS/Azure/GCP)
- Using CLI commands directly (aws cli, az, gcloud)
- Emergency fixes bypassing Terraform
- Other teams making changes
- Automated scripts or processes
- Cloud provider updates/patches

## 3. Why Drift is Bad

- Code no longer represents reality
- State file becomes unreliable
- Future terraform applies may fail or behave unexpectedly
- Hard to troubleshoot issues
- Security/compliance risks from untracked changes
- Team confusion about actual setup
- Documentation becomes inaccurate

## 4. Detecting Drift

    # See differences between code and reality
    terraform plan

    # Update state file to match real infrastructure (safe, no changes)
    terraform apply -refresh-only

## 5. Fixing Drift

**Option 1: Code was wrong, reality is correct**
- Update your .tf files to match reality
- Run `terraform apply`

**Option 2: Reality is wrong, code is correct**
- Just run `terraform apply` to fix infrastructure

**Option 3: Import unmanaged resources**

    terraform import aws_instance.example i-1234567890

## 6. Preventing Drift

- Golden Rule: ALL changes through Terraform only
- Lock down manual access with IAM policies
- Use CI/CD pipelines for changes
- Schedule regular drift checks (daily/weekly)
- Enable cloud audit logs
- Educate team on proper workflow
- Use remote state with locking

## 7. Best Practices

- Check for drift regularly with terraform plan
- Use `terraform apply -refresh-only` to sync state safely
- Keep state files secure (use remote backends)
- Never edit state files manually
- Tag resources as "managed by Terraform"
- Code review all Terraform changes
- Document drift response procedures

---