# CI/CD Pipeline for Static Website on AWS EC2 Using GitHub Actions

**Overview**

This document provides a detailed guide to deploy a static website hosted on an AWS EC2 instance with Nginx. The website source code resides in GitHub and each push to the main branch automatically updates the site using GitHub Actions.

## 1. Create an EC2 Instance

1. Choose **Amazon Linux AMI**
2. Instance type: **t3.micro**
3. Create/choose key pair (PEM)
4. Allow:
    - SSH (22)
    - HTTP (80)
5. Launch the instance

## 2. Install Nginx

1. SSH into the server
2. Install nginx

        sudo yum install nginx -y
        sudo systemctl enable nginx
        sudo systemctl start nginx

3. http://EC2_PUBLIC_IP -> the Nginx welcome page is displayed.

## 3. Clone Static Website Into Nginx Directory

1. Nginx serves files from:

        /usr/share/nginx/html


2. Clean it first:

        sudo rm -rf /usr/share/nginx/html/*


3. Clone the static-website repo:

        cd /usr/share/nginx/html
        sudo git clone https://github.com/itsknyy/static-website-ci-cd.git .

## 4. Prepare Existing SSH Key for GitHub Actions (Base64)

Use the same SSH key used to log in to EC2.  
**NOTE:** (This is okay for personal projects, not recommended for production — better to use a separate deploy key.)

1. Base64 Encode the Key
- GitHub Secrets require single-line values, but SSH key is multi-line.
- Base64 converts it to one safe line without corrupting it.

    base64 key.pem

2. Add GitHub Secrets  

- Go to:  
**Repo → Settings → Secrets → Actions**

- Add:  
Secret: SSH_KEY64 -> (base64 encoded key)  
variable: SERVER_IP -> (Public-IP of ec2)

## 5. Create GitHub Actions Workflow

- Inside static-website repo create:

    .github/workflows/deploy.yml

- Workflow

        name: Update index.html in my server.

        on: 
        push:
            branches:
            - main

        jobs:
        deploymycode:
            name: "Deploy my code in my server"
            runs-on: ubuntu-latest
            env:
            SERVER_IP: ${{ vars.SERVER_IP }}

            steps: 
            - uses: actions/checkout@v4    
                # with: 
                # fetch-depth: 0
            
            - name: "Configure SSH"
                run: |     #pipe is used for multi-line command
                mkdir -p ~/.ssh
                chmod 700 ~/.ssh
                echo -e "Host *\n\tStrictHostKeyChecking no\n\n" > ~/.ssh/config  
                cat ~/.ssh/config   #to verify
                touch ~/.ssh/known_hosts
                chmod 600 ~/.ssh/known_hosts
            
            - name: "SSH Key Access" #ci
                run: |
                touch mykey.pem
                echo "$SSH_KEY64" | base64 -d > mykey.pem && chmod 400 mykey.pem
                ssh-keygen -R $SERVER_IP
                
                env:
                SSH_KEY64: ${{ secrets.SSH_KEY64 }}

            - name: "Depoly the code to my server"
                run: |      # -t means pseudo terminal
                ssh -t ec2-user@$SERVER_IP -i mykey.pem 'cd /usr/share/nginx/html && git pull origin main'


## 6. Test the CI/CD Setup

- Make a change to the site
- git add . && git commit -m "update"
- git push
- Go to GitHub → Actions
- Wait for the pipeline to complete

---

