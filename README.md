# Complete Docker Commands Cheat Sheet

## What is Docker?
Docker is a containerization platform that packages applications and their dependencies into lightweight, portable containers. It ensures consistent deployment across different environments, eliminates "it works on my machine" issues, and provides efficient resource utilization.

## Basic Image Operations

==> Pull Images
```bash
# Pull latest version of an image
$ docker pull nginx

# Pull specific version
$ docker pull nginx:1.21-alpine

# Pull from different registry
$ docker pull mcr.microsoft.com/dotnet/core/aspnet:3.1
```

==> List Images
```bash
# List all images
$ docker images

# List images with specific format
$ docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}"

# List image IDs only
$ docker images -q
```

==> Remove Images
```bash
# Remove single image
$ docker rmi nginx

# Remove image by ID
$ docker rmi 1a2b3c4d5e6f

# Remove all unused images
$ docker image prune

# Force remove image (even if containers exist)
$ docker rmi -f nginx
```

## Container Operations

==> Run Containers
```bash
# Basic run
$ docker run nginx

# Run in interactive mode
$ docker run -it ubuntu /bin/bash

# Run in detached mode
$ docker run -d nginx

# Run with custom name
$ docker run --name my-nginx -d nginx

# Run with port mapping
$ docker run -p 8080:80 -d nginx

# Run with environment variables
$ docker run -e MYSQL_ROOT_PASSWORD=password -d mysql:8.0

# Run with volume mount
$ docker run -v /host/path:/container/path -d nginx

# Run with resource limits
$ docker run --memory="512m" --cpus="1.5" -d nginx
```

==> List Containers
```bash
# List running containers
$ docker ps

# List all containers (running + stopped)
$ docker ps -a

# List container IDs only
$ docker ps -q

# List with custom format
$ docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"
```

==> Container Lifecycle
```bash
# Start stopped container
$ docker start my-nginx

# Stop running container
$ docker stop my-nginx

# Restart container
$ docker restart my-nginx

# Pause container
$ docker pause my-nginx

# Unpause container
$ docker unpause my-nginx

# Kill container (force stop)
$ docker kill my-nginx
```

==> Remove Containers
```bash
# Remove stopped container
$ docker rm my-nginx

# Force remove running container
$ docker rm -f my-nginx

# Remove all stopped containers
$ docker container prune

# Remove container after it stops
$ docker run --rm -it ubuntu /bin/bash
```

## Container Inspection & Debugging

==> View Logs
```bash
# View container logs
$ docker logs my-nginx

# Follow logs in real-time
$ docker logs -f my-nginx

# View last 100 lines
$ docker logs --tail 100 my-nginx

# View logs with timestamps
$ docker logs -t my-nginx
```

==> Execute Commands in Container
```bash
# Access bash shell
$ docker exec -it my-nginx /bin/bash

# Run single command
$ docker exec my-nginx ls -la /etc

# Run as different user
$ docker exec -u root -it my-nginx /bin/bash
```

==> Inspect Container
```bash
# Get detailed container info
$ docker inspect my-nginx

# Get specific info (IP address)
$ docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' my-nginx

# View container stats
$ docker stats my-nginx

# View running processes
$ docker top my-nginx
```

## Docker Networking

==> Network Operations
```bash
# List networks
$ docker network ls

# Create network
$ docker network create my-network

# Create network with subnet
$ docker network create --subnet=172.20.0.0/16 my-network

# Remove network
$ docker network rm my-network

# Connect container to network
$ docker network connect my-network my-nginx

# Disconnect container from network
$ docker network disconnect my-network my-nginx

# Inspect network
$ docker network inspect my-network
```

==> Run Container with Network
```bash
# Run container in specific network
$ docker run --network my-network -d nginx

# Run with custom hostname
$ docker run --network my-network --hostname web-server -d nginx
```

## Docker Volumes

==> Volume Operations
```bash
# List volumes
$ docker volume ls

# Create volume
$ docker volume create my-volume

# Remove volume
$ docker volume rm my-volume

# Remove unused volumes
$ docker volume prune

# Inspect volume
$ docker volume inspect my-volume
```

==> Using Volumes
```bash
# Mount named volume
$ docker run -v my-volume:/app/data -d nginx

# Mount host directory (bind mount)
$ docker run -v /host/path:/container/path -d nginx

# Mount with read-only
$ docker run -v my-volume:/app/data:ro -d nginx

# Create and mount volume in one command
$ docker run -v my-new-volume:/data -d nginx
```

# Docker Compose

==> Basic Compose Commands
```bash
# Start services (detached mode)
$ docker-compose up -d

# Start with specific file
$ docker compose -f docker-compose.yml up -d

# View running services
$ docker-compose ps

# View logs
$ docker-compose logs

# Stop services
$ docker-compose down

# Stop and remove volumes
$ docker-compose down -v

# Rebuild and start
$ docker-compose up --build -d

# Scale service
$ docker-compose up --scale web=3 -d
```

==> Example docker-compose.yml
```yaml
version: '3.8' # it is ooptional for current versions of docker.
services:
  web:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./html:/usr/share/nginx/html
  
  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: password
    volumes:
      - db_data:/var/lib/mysql

volumes:
  db_data:
```

## Building Images (Dockerfile)

==> Basic Dockerfile Commands
```dockerfile
# Example Dockerfile
FROM node:16-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```

==> Build Commands
```bash
# Build image from Dockerfile
$ docker build -t my-app .

# Build with different Dockerfile
$ docker build -f Dockerfile.prod -t my-app:prod .

# Build with build args
$ docker build --build-arg NODE_ENV=production -t my-app .

# Build without cache
$ docker build --no-cache -t my-app .
```

## System Cleanup

==> Cleanup Commands
```bash
# Remove unused containers, networks, images
$ docker system prune

# Remove everything including volumes
$ docker system prune -a --volumes

# View disk usage
$ docker system df

# Remove all stopped containers
$ docker container prune

# Remove all unused images
$ docker image prune -a

# Remove all unused volumes
$ docker volume prune

# Remove all unused networks
$ docker network prune
```

## Additional Useful Commands

==> Copy Files
```bash
# Copy from container to host
$ docker cp my-nginx:/etc/nginx/nginx.conf ./nginx.conf

# Copy from host to container
$ docker cp ./app.js my-nginx:/usr/share/nginx/html/
```

==> Save/Load Images
```bash
# Save image to tar file
$ docker save -o nginx.tar nginx:latest

# Load image from tar file
$ docker load -i nginx.tar

# Export container to tar
$ docker export my-nginx > nginx-container.tar

# Import container from tar
$ docker import nginx-container.tar my-nginx:latest
```

==> Docker Hub Operations
```bash
# Login to Docker Hub
$ docker login

# Tag image for push
$ docker tag my-app:latest username/my-app:latest

# Push to Docker Hub
$ docker push username/my-app:latest

# Pull from private registry
$ docker pull myregistry.com/my-app:latest
```

## Pro Tips 

1. **Consistency**: Docker ensures your app runs the same everywhere
2. **Efficiency**: Containers share the OS kernel, making them lightweight
3. **Scalability**: Easy to scale applications horizontally
4. **DevOps**: Perfect for CI/CD pipelines
5. **Microservices**: Ideal for microservices architecture
