# Terraform Workspaces

# 1. Workspaces

Workspaces allow you to manage multiple instances of infrastructure from the same configuration. Each workspace mainatins its own seperate file.

**1.1. Default Behaviour**
- Terraform starts with a single workspace called "default"
- All state is stored in this workspace unless you create and switch to others.

**1.2. Common Commands**

    terraform workspace list          # Show all workspaces
    terraform workspace new dev       # Create new workspace
    terraform workspace select dev    # Switch to workspace
    terraform workspace show          # Show current workspace
    terraform workspace delete dev    # Delete workspace

**1.3. Use Cases**

- Separate environments (dev, staging, production)
- Testing changes without affecting main infrastructure
- Managing similar infrastructure for different teams or projects

**1.4. Important Notes**

- Workspaces use the same configuration files
- State files are completely isolated between workspaces
- Variables and resources remain the same; only state differs

---