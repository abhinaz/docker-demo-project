Docker Demo Project Documentation
Overview
This project demonstrates how to containerize a Django Todo application using Docker. The application is deployed in a Docker container and exposed through port 8001 for external access.
Repository

GitHub Repository: https://github.com/abhinaz/docker-demo-project.git
Application Type: Django Todo App
Container Technology: Docker

Prerequisites

Ubuntu system (18.04 or later)
Terminal/Command line access
Internet connection for downloading Docker and cloning repository

Project Setup Guide
Step 1: Install Docker on Ubuntu
1.1 Update System Packages
bashsudo apt update
sudo apt upgrade -y
1.2 Remove Old Docker Versions
Remove any existing Docker installations to avoid conflicts:
bashsudo apt remove docker docker-engine docker.io containerd runc
1.3 Install Required Dependencies
bashsudo apt install apt-transport-https ca-certificates curl software-properties-common -y
1.4 Add Docker's GPG Key
bashcurl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker.gpg
1.5 Add Docker's Official Repository
bashecho \
"deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
1.6 Install Docker Engine
bashsudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io -y
1.7 Verify Installation
bashdocker --version
1.8 (Optional) Enable Docker Without Sudo
To run Docker commands without sudo:
bashsudo usermod -aG docker $USER
Note: Log out and log back in for this change to take effect.
Step 2: Project Setup
2.1 Create Project Directory
bashmkdir project
cd project
2.2 Clone the Repository
bashgit clone https://github.com/abhinaz/docker-demo-project.git
cd docker-demo-project
2.3 Check Running Containers
Verify no conflicting containers are running:
bashdocker ps
Step 3: Build and Deploy
3.1 Build Docker Image
Create the Docker image from the Dockerfile:
bashdocker build . -t todo-app
Command Breakdown:

docker build: Command to build a Docker image
.: Current directory context (where Dockerfile is located)
-t todo-app: Tags the image with name "todo-app"

3.2 Run the Container
Deploy the application in a Docker container:
bashdocker run -d --name todo-django-ctr -p 8001:8001 todo-app:latest
Command Breakdown:

docker run: Command to create and start a container
-d: Run container in detached mode (in background)
--name todo-django-ctr: Assign name "todo-django-ctr" to the container
-p 8001:8001: Port mapping (host:container)

Maps host port 8001 to container port 8001
External traffic on port 8001 forwards to container's port 8001


todo-app:latest: Use the "todo-app" image with "latest" tag

Step 4: Configure Security Group (AWS EC2)
If deploying on AWS EC2, configure security group for external access:

Access AWS Console: Navigate to EC2 Dashboard
Select Instance: Choose your EC2 instance
Edit Security Group:

Click on Security tab
Click on Security Group link


Edit Inbound Rules:

Click "Edit inbound rules"
Add new rule:

Type: Custom TCP
Port Range: 8001
Source: Anywhere (0.0.0.0/0)


Save rules



Step 5: Access the Application
Once deployed, access the Todo application:

Local Access: http://localhost:8001
EC2 Access: http://YOUR_EC2_PUBLIC_IP:8001

Common Docker Commands
Container Management
bash# List running containers
docker ps

# List all containers (including stopped)
docker ps -a

# Stop a container
docker stop todo-django-ctr

# Start a stopped container
docker start todo-django-ctr

# Remove a container
docker rm todo-django-ctr

# View container logs
docker logs todo-django-ctr
Image Management
bash# List images
docker images

# Remove an image
docker rmi todo-app

# Pull an image from registry
docker pull <image-name>
Troubleshooting
Common Issues

Permission Denied

Solution: Add user to docker group or use sudo


Port Already in Use

Check running processes: netstat -tlnp | grep :8001
Stop conflicting services or use different port


Build Failures

Ensure Dockerfile exists in current directory
Check Docker daemon is running: sudo systemctl status docker


Container Won't Start

Check logs: docker logs todo-django-ctr
Verify image exists: docker images



Useful Debug Commands
bash# Execute commands inside running container
docker exec -it todo-django-ctr /bin/bash

# Inspect container details
docker inspect todo-django-ctr

# Monitor container resource usage
docker stats todo-django-ctr
