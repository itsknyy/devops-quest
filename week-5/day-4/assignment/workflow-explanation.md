## What the workflow does

Every time you push to the main branch, GitHub Actions will:
- Build a Docker image for an NGINX app
- Copy that Docker image to your EC2 server
- Run it on EC2

--- 

### 1. Trigger on push to main

    on:
      push:
        branches:
          - main

- The workflow runs only when you push code to the main branch.

### 2. Run job on Ubuntu machine

    runs-on: ubuntu-latest

- GitHub Actions gives you a fresh Ubuntu virtual machine to run the steps.

### 3. Load EC2 IP from GitHub Variables

    env:
      SERVER_IP: ${{ vars.SERVER_IP }}

### 4. Checkout repository

    uses: actions/checkout@v4

- Downloads your GitHub repo into the workflow machine so Docker can build it.

### 5. Build Docker image

    docker build -t mynginxapp .

- Builds Docker image from the Dockerfile.
- Tags it as mynginxapp.

### 6. Save the Docker image into a tar file

    docker save mynginxapp:latest -o mynginxapp.tar

- Converts the Docker image into a .tar file.
- This file can be transferred to  EC2 instance.

### 7. Configure SSH (disable host key checking)

    StrictHostKeyChecking no

- Creates SSH folder
- Disables the need to confirm "Are you sure you want to connect?"
- Makes automated SSH connection possible.

### 8. Add private key from GitHub Secrets

    echo "$SSH_KEY64" | base64 -d > mykey.pem
    chmod 400 mykey.pem

- Takes the EC2 private key stored in GitHub Secrets
- Decodes it
- Saves it as mykey.pem
- Makes it readable only by the owner (required by SSH)

### 9. Upload the Docker image to EC2

    scp -i mykey.pem mynginxapp.tar ec2-user@$SERVER_IP:/home/ec2-user/

- Uses SCP to copy the .tar file to your EC2 instance.

### 10. SSH into EC2 and deploy container

    ssh -i mykey.pem ec2-user@$SERVER_IP "
      docker load -i mynginxapp.tar &&
      docker stop mynginxapp || true &&
      docker rm mynginxapp || true &&
      docker run -d -p 80:80 --name mynginxapp mynginxapp:latest
    "

Inside EC2, it does
- Load the Docker image from the tar file
- Stop the old container (if running – ignore errors with || true)
- Remove the old container
- Run a fresh container from the new image
- Expose port 80 -> 80

---
