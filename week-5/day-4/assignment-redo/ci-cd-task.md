# TASK: Write a CI/CD in GitHub Actions that triggers when code is changed in your repo. It should build a docker image and deploy that in EC2.

**Overview**

This document describes how to automatically deploy a Dockerized application to an EC2 instance using GitHub Actions. Each push to the main branch triggers a build and deployment to the server.

## 1. Create an EC2 Instance

1. Choose **Amazon Linux AMI**
2. Instance type: **t3.micro**
3. Create/choose key pair (PEM)
4. Allow:
    - SSH (22)
    - HTTP (80)
5. Launch the instance

## 2. Install Docker & Docker compose

1. SSH into the server
2. Install docker
    
        sudo dnf update -y
        sudo dnf install docker -y
        sudo systemctl start docker
        sudo usermod -aG docker ec2-user

3. Install Docker Compose

## 3. Prepare GitHub

1. Add secrets: base64 encoded KEY
2. Add secrets: dockerhub username
3. Add secrets: dockerhub token
4. Add variable: EC2 Public IP
5. Add variable: docker repo name

## 4. Add Dockerfile into github repo

    FROM nginx:alpine

    RUN rm -rf /usr/share/nginx/html/*

    COPY . /usr/share/nginx/html/

    EXPOSE 80

## 5. Create docker-compose.yml on EC2

SSH again into EC2:

    cd /home/ec2-user
    nano docker-compose.yml
    ---
    version: "3"

    services:
    web:
        image: yourdockerhubusername/yourrepo:latest
        container_name: web
        ports:
        - "80:80"
        restart: always


## 6. Add GitHub Actions Workflow

    name: CI/CD TO EC2 (DOCKERHUB)

    on:
    push:
        branches:
        - main

    jobs:
    deploy:
        runs-on: ubuntu-latest

        env:
        SERVER_IP: ${{ vars.SERVER_IP }}
        DOCKER_REPO: ${{vars.DOCKER_REPO}}

        steps:
        # 1. Checkout repo
        - name: Checkout code
            uses: actions/checkout@v4

        # 2. Login to dockerhub
        - name: Login to Docker Hub
            uses: docker/login-action@v3
            with:
            username: ${{ secrets.DOCKERHUB_USERNAME }}
            password: ${{ secrets.DOCKERHUB_TOKEN }}

        # 3. Build Docker Image
        - name: Build Docker Image
            run: |
            docker build -t ${{ secrets.DOCKERHUB_USERNAME }}/$DOCKER_REPO:latest .


        # 4. Push image to Docker Hub
        - name: Push Docker image
            run: |
            docker push ${{ secrets.DOCKERHUB_USERNAME }}/$DOCKER_REPO:latest

        # 4. Prepare SSH & disable host key checking
        - name: Configure SSH
            run: |
            mkdir -p ~/.ssh
            chmod 700 ~/.ssh
            echo -e "Host *\n\tStrictHostKeyChecking no\n\n" > ~/.ssh/config
            chmod 600 ~/.ssh/config

        # 5. Add private key from GitHub Secrets
        - name: Add SSH Key
            env:
            SSH_KEY64: ${{ secrets.SSH_KEY64 }}
            run: |
            echo "$SSH_KEY64" | base64 -d > mykey.pem
            chmod 400 mykey.pem

        # 7. Load & run container on EC2
        - name: Deploy via Docker Compose on EC2
            run: |
            ssh -i mykey.pem ec2-user@$SERVER_IP "
                cd /home/ec2-user/app &&
                docker compose pull &&
                docker compose up -d
            "

## 7. Test the CI/CD Setup

**1. Push code -> GitHub Actions runs automatically.** 

**2. GitHub Actions Triggered**

![image](../images/image-7.png)
---
**3. Docker Image Loaded on EC2**

![image](../images/image-8.png)
---
**4. Application Running in Browser**

![image](../images/image-9.png)
---

## 8. Test the update

**1. GitHub Actions Triggered**

![image](../images/image-10.png)
---
**2. Updated Application Running in Browser**

![image](../images/image-11.png)

---

