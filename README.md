Introduction to Docker & Containerization

A Complete Guide to Understanding & Using Docker Effectively

 Overview

What is Docker & Why Do We Need It?

Containerization is a revolutionary approach to application deployment, allowing software to run in isolated environments without the overhead of full Virtual Machines (VMs). Docker is the leading container runtime that simplifies container management, making applications lightweight, portable, and scalable across different environments.

This document provides a technical deep dive into Docker, including:
✅ How Docker compares to Virtual Machines (VMs)
✅ Hands-on setup guide for Docker
✅ Building & managing containers with Docker commands
✅ Dockerfile tutorial to create custom images
✅ Multi-container setup using Docker Compose

Docker vs. Virtual Machines (VMs)

🛠️ Virtual Machines vs. Containers: Key Differences

Feature

Virtual Machines (VMs)

Containers

Isolation

Hardware-level

OS-level

OS Requirement

Each VM has a full OS

Shared host OS kernel

Startup Time

Slow (minutes)

Fast (seconds)

Resource Consumption

High (each VM has its own OS)

Low (only processes + dependencies)

Portability

Harder to move VMs across environments

Easy to package & deploy anywhere

Scalability

Limited by VM overhead

Highly scalable

Security

Strong isolation but requires patches

Secure, but kernel vulnerabilities affect all containers

 Visual Representation

Traditional Virtual Machine Architecture

[ Hardware ]
    ⬆
[ Hypervisor (VMware, VirtualBox, etc.) ]
    ⬆
[ VM 1 ]  [ VM 2 ]  [ VM 3 ]
[ OS ]    [ OS ]    [ OS ]
[ App ]   [ App ]   [ App ]


Each VM runs its own OS, increasing resource consumption.

Slow boot times due to OS initialization.

Docker Container Architecture

[ Hardware ]
    ⬆
[ OS (Linux/Windows) ]
    ⬆
[ Docker Engine ]
    ⬆
[ Container 1 ]  [ Container 2 ]  [ Container 3 ]
[ App ]         [ App ]         [ App ]


No separate OS for each container, just the app and dependencies.

Containers boot in seconds since they share the host OS kernel.

Hands-On: Setting Up Docker on an EC2 Instance

 Installing Docker on Ubuntu (AWS EC2)

Step 1: Launch an EC2 Instance

Choose Ubuntu 20.04 or Amazon Linux 2

Allow inbound traffic on ports 22 (SSH) and 8080 (for web applications)

Step 2: Install Docker

Run the following commands on your EC2 instance:

sudo apt update && sudo apt install -y docker.io
sudo systemctl start docker
sudo systemctl enable docker

Verify installation:

docker --version

Step 3: Run a Sample Container

docker run -d -p 8080:80 nginx

This command runs an NGINX web server inside a container.

Access it via http://<EC2-Public-IP>:8080

Step 4: Managing Containers

docker ps                # List running containers  
docker stop <container_id>  # Stop a container  
docker rm <container_id>    # Remove a container  
docker images            # List downloaded images  
docker rmi <image_id>    # Remove an image  

Dockerfile Tutorial: Creating Custom Images

A Dockerfile is a script containing instructions to build a custom container image.

📌 Example: Custom NGINX Server

1️⃣ Create a new directory:

mkdir my-nginx
cd my-nginx

2️⃣ Create a Dockerfile:

# Use the official NGINX image
FROM nginx:latest 

# Copy custom HTML file
COPY index.html /usr/share/nginx/html/index.html

# Expose port 80
EXPOSE 80

3️⃣ Create an index.html file:

<html>
  <head><title>Welcome to My Custom NGINX!</title></head>
  <body><h1>Hello from a Docker Container!</h1></body>
</html>


4️⃣ Build & Run the Custom Image:

docker build -t my-custom-nginx .
docker run -d -p 8080:80 my-custom-nginx

 Now visit http://<EC2-Public-IP>:8080 to see your custom page!

Multi-Container Setup with Docker Compose

Docker Compose allows us to run multiple containers together using a single docker-compose.yml file.

Example: Running WordPress + MySQL

1️⃣ Create a docker-compose.yml file:

version: '3.1'

services:
  db:
    image: mysql:5.7
    container_name: mysql_db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpass
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wpuser
      MYSQL_PASSWORD: wppass

  wordpress:
    image: wordpress:latest
    container_name: wordpress_site
    restart: always
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: wpuser
      WORDPRESS_DB_PASSWORD: wppass


2️⃣ Start the Multi-Container Application:

docker-compose up -d

3️⃣ Access WordPress:
Go to http://<EC2-Public-IP>:8080 and complete the WordPress setup.

4️⃣ Stop & Remove Containers:

docker-compose down

Summary & Key Takeaways

✅ Containers provide lightweight isolation without needing separate OS instances
✅ Docker simplifies container management with images & automation tools
✅ Multi-container setups (e.g., WordPress + MySQL) are easy with Docker Compose
✅ You can build, run, and scale applications efficiently using Docker

📚 Additional Resources

🔹 Docker Documentation: https://docs.docker.com/ 
🔹 Docker Hub (Public Image Registry): https://hub.docker.com/ 
🔹 Kubernetes for Container Orchestration: https://kubernetes.io/ 

🚀 You're now ready to master Docker! Need help? Let’s discuss in the comments! 😊
