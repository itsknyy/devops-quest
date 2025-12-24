# Terraform Workflow & State Management

## Terraform Workflow

This section documents what each Terraform command does.

### 1. Writing Terraform Configuration

`.tf` files are created to define providers, resources, variables, and modules.

---

### 2. Initializing Terraform (`terraform init`)

`terraform init` downloads required provider plugins and sets up backend configuration.

---

### 3. Validating Configuration (`terraform validate`)

`terraform validate` checks whether the syntax and structure of the configuration are correct.

---

### 4. Formatting Code (`terraform fmt`)

`terraform fmt` automatically formats Terraform files using the standard style.

---

### 5. Previewing Changes (`terraform plan`)

`terraform plan` compares the configuration with the existing infrastructure and shows what will be created, updated, or destroyed.

---

### 6. Applying Changes (`terraform apply`)

`terraform apply` performs the actions shown in the plan and updates actual cloud infrastructure.

---

### 7. Reviewing Infrastructure (`terraform show`)

`terraform show` displays the current state of infrastructure as Terraform understands it.

--- 

### 8. Updating Resources

After modifying `.tf` files, run:

    terraform plan
    terraform apply

These commands calculate changes and apply them.

---

### 9. Destroying Infrastructure (`terraform destroy`)

`terraform destroy` removes all resources created by the Terraform configuration.

---

## Terraform State Management 

Terraform uses a state file to track managed infrastructure.

### What is Terraform State?

Terraform stores metadata about infrastructure in **terraform.tfstate**, including:

* Resource IDs
* Attributes
* Dependencies
* Mappings between configuration and real resources

### Why State Matters?

State is used to:

* Understand existing resources
* Apply only necessary updates
* Prevent accidental recreation of resources

### Common State Commands

    terraform state list       # Shows all resources tracked in state
    terraform state show <id>  # Displays details for a specific resource
    terraform state rm <id>    # Removes a resource from state tracking
    terraform state mv ...     # Moves or renames resources in state

---