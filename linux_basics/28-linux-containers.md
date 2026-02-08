# Linux Containers (Docker & Podman)
# ----------------------------------
# This lesson introduces containerization using:
#   - Docker
#   - Podman (daemonless alternative)
#   - images, containers, volumes, networks
#   - Dockerfiles
#   - container lifecycle
# All commands include # comments for beginners.

# ---------------------------------------------------------------
# 1. What Are Containers?
# ---------------------------------------------------------------
# Containers package applications with:
#   - dependencies
#   - libraries
#   - configuration
#
# Benefits:
#   - lightweight
#   - fast startup
#   - reproducible environments
#   - isolated from host system

# ---------------------------------------------------------------
# 2. Install Docker (Ubuntu)
# ---------------------------------------------------------------

sudo apt update
sudo apt install docker.io -y

## Enable and start Docker
sudo systemctl enable docker
sudo systemctl start docker

## Check version
docker --version

## Run Docker without sudo
sudo usermod -aG docker $USER

# ---------------------------------------------------------------
# 3. Basic Docker Commands
# ---------------------------------------------------------------

## List images
docker images

## List running containers
docker ps

## List all containers
docker ps -a

## Pull an image
docker pull nginx

## Run a container
docker run nginx

## Run in background
docker run -d nginx

## Run with port mapping
docker run -d -p 8080:80 nginx

## Stop container
docker stop <container-id>

## Remove container
docker rm <container-id>

## Remove image
docker rmi nginx

# ---------------------------------------------------------------
# 4. Inspecting Containers
# ---------------------------------------------------------------

## View logs
docker logs <container-id>

## Enter container shell
docker exec -it <container-id> bash

## Inspect container details
docker inspect <container-id>

# ---------------------------------------------------------------
# 5. Docker Volumes (Persistent Storage)
# ---------------------------------------------------------------

## Create volume
docker volume create mydata

## List volumes
docker volume ls

## Use volume in container
docker run -d -p 8080:80 -v mydata:/var/www/html nginx

## Inspect volume
docker volume inspect mydata

# ---------------------------------------------------------------
# 6. Docker Networks
# ---------------------------------------------------------------

## List networks
docker network ls

## Create network
docker network create mynet

## Run container on network
docker run -d --network=mynet nginx

## Inspect network
docker network inspect mynet

# ---------------------------------------------------------------
# 7. Dockerfile (Build Your Own Image)
# ---------------------------------------------------------------

## Example Dockerfile:
# FROM ubuntu:22.04
# RUN apt update && apt install -y nginx
# COPY index.html /var/www/html/
# CMD ["nginx", "-g", "daemon off;"]

## Build image
docker build -t mynginx .

## Run custom image
docker run -d -p 8080:80 mynginx

# ---------------------------------------------------------------
# 8. Docker Compose (Multi-Container Apps)
# ---------------------------------------------------------------

## Example docker-compose.yml:
# version: "3"
# services:
#   web:
#     image: nginx
#     ports:
#       - "8080:80"
#   db:
#     image: mysql
#     environment:
#       MYSQL_ROOT_PASSWORD: secret

## Start services
docker compose up -d

## Stop services
docker compose down

# ---------------------------------------------------------------
# 9. Podman (Docker Alternative)
# ---------------------------------------------------------------
# Podman is daemonless and rootless by default.

## Install Podman
sudo apt install podman -y

## Check version
podman --version

## Run container
podman run -d nginx

## Podman is Docker-compatible:
podman images
podman ps

## Build image
podman build -t myapp .

# ---------------------------------------------------------------
# 10. Podman vs Docker
# ---------------------------------------------------------------

## Docker:
# - daemon-based
# - requires root unless configured
# - widely used in production

## Podman:
# - daemonless
# - rootless containers
# - Docker-compatible CLI

# ---------------------------------------------------------------
# 11. Container Cleanup
# ---------------------------------------------------------------

## Remove all stopped containers
docker container prune -f

## Remove unused images
docker image prune -f

## Remove everything
docker system prune -a -f

# ---------------------------------------------------------------
# 12. Practical Examples
# ---------------------------------------------------------------

## 1. Run a Python app
docker run -d -p 5000:5000 python:3.10

## 2. Serve static website
docker run -d -p 8080:80 -v $(pwd)/site:/usr/share/nginx/html nginx

## 3. Build and run custom app
docker build -t myapp .
docker run -d -p 3000:3000 myapp

## 4. Rootless Podman container
podman run -d -p 8080:80 nginx

# ---------------------------------------------------------------
# 13. Summary
# ---------------------------------------------------------------
# - Docker → container engine
# - Podman → rootless alternative