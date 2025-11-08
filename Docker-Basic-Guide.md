# Docker Basic Guide

## Index
1. [Installation on ubuntu](#installation-on-ubuntu)
2. [Images](#images)
3. [Containers](#containers)
4. [Volume](#volume)
5. [Network](#network)
6. []()
7. []()
----------------------------

## Installation on ubuntu
**!Important:** First of all two things to note:
1. Create an acount on [Docker Hub](https://hub.docker.com/) to download images and manage your repositories.
2. **Install Docker with docker-desktop for better experience.**

ok si y yo se que todos somos super hacker y que `"nooo yo me instalo por terminal porque soy como mi idolo Neo el elegido"`. Pero lo que me paso a mi es que instale **Docker** primero por terminal, y despues cuando instale **docker-desktop** y genere y configure la GPG key y etc etc.

Aprendo luego de intentar borrar 500 veces el hello-world que el docker de la terminal y el docker-desktop son cosas distintas y no se comunican entre si, y aunque si se puede configurar para que juntos y bla bla, lo que encontre no me funciono y perdi mucho tiempo. Entonces tuve que desinstalar todo y volver a instalar **docker-desktop**. Asi que les recomiendo instalarlo directamente con **docker-desktop**.**
la otra ventaja que tiene docker-desktop es que para correrlo por terminal no necesitas usar sudo cada vez que quieras correr un comando de docker por lo tanto no hay que estar poniendo la contraseña a cada rato.

There are two ways to install Docker on Ubuntu:
### By apt package manager
To install Docker on Ubuntu, follow these steps:
```bash
# 1. Update your existing list of packages:
sudo apt update

# 2. Install prerequisite packages:
sudo apt install apt-transport-https ca-certificates curl software-properties-common

# 3. Add Docker’s official GPG key:
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# 4. Add the Docker repository to APT sources:
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# 5. Update the package database with Docker packages from the newly added repo:
sudo apt update

# 6. Make sure you are about to install from the Docker repo instead of the default Ubuntu repo:
apt-cache policy docker-ce

# 7. Finally, install Docker:
sudo apt install docker-ce

# 8. Docker should now be installed, the daemon started, and the process enabled to start on  boot. Verify that Docker is installed correctly by running:
sudo systemctl status docker
```
### By docker-desktop deb package
To install Docker Desktop on Ubuntu using the .deb package, follow these steps:
```bash
# 1. Update your existing list of packages:
sudo apt update

# 2. Install prerequisite packages:
sudo apt install apt-transport-https ca-certificates curl software-properties-common

# 3. Download the Docker Desktop .deb package from the official Docker website:
curl -LO https://desktop.docker.com/linux/main/amd64/docker-desktop-<
VERSION>_amd64.deb

# 4. Change file permissions to make the .deb package executable:
chmod +x docker-desktop-<VERSION>_amd64.deb

# 5. Install the downloaded .deb package using dpkg:
sudo dpkg -i docker-desktop-<VERSION>_amd64.deb

# 6. If there are any dependency issues, fix them by running:
sudo apt --fix-broken install

# 7. Start Docker Desktop:
systemctl --user start docker-desktop

# 8. Enable Docker Desktop to start on login:
systemctl --user enable docker-desktop

# 9. Verify that Docker Desktop is running:
systemctl --user status docker-desktop
```
----------------
## Images
Docker images are the building blocks of Docker containers. They are lightweight, standalone, and executable software packages that include everything needed to run a piece of software, including the code, runtime, libraries.

### Common Docker Image Commands
- `docker image pull <image_name>`: Download a Docker image from a registry (e.g., Docker Hub).
- `docker imge ls`: List all Docker images on the local machine.
- `docker image rm <image_name>`: Remove a Docker image from the local machine.
- `docker image inspect <image_name>`: Display detailed information about a Docker image.
- `docker image build -t <image_name> <path>`: Build a Docker image from a Dockerfile located at the specified path.

```bash
# Example: Pulling an Ubuntu image from Docker Hub
docker image pull mariadb:jammy

# Example: Listing all Docker images
docker image ls
# REPOSITORY   TAG            IMAGE ID       CREATED         SIZE
# mariadb      jammy          e101f9db3191   17 months ago   551MB

# Example: Removing a Docker image
docker image rm mariadb:jammy

# Or Remove by IMAGE ID example:
docker image rm e10
```
----------------

## Containers
Docker containers are lightweight, standalone, and executable software packages that encapsulate an application and its dependencies. Containers are created from Docker images and provide a consistent runtime environment for applications, ensuring that they run the same way regardless of where they are deployed.

### Common Docker Container Commands
- `docker container run <image_name>`: Create and start a new container from a specified image.
- `docker container ls`: List all running containers.
- `docker container ls -a`: List all containers, including stopped ones.
- `docker container stop <container_id>`: Stop a running container.
- `docker container rm <container_id>`: Remove a stopped container.
- `docker container inspect <container_id>`: Display detailed information about a container.
- `docker container logs <container_id>`: View the logs of a container.
```bash

# Example: Running a new container from the Ubuntu image
docker container run mariadb:jammy

# This will start a new container and run the default command specified in the image.

# Example: Listing all running containers
docker container ls

# CONTAINER ID   IMAGE          COMMAND                  CREATED       STATUS       PORTS                                         NAMES
# 084713bdc9e4   mariadb:jammy  "docker-entrypoint.s…"   6 hours ago   Up 6 hours   0.0.0.0:3306->3306/tcp, [::]:3306->3306/tcp   world-db

# Example: Listing all containers, including stopped ones
docker container ls --all

# CONTAINER ID   IMAGE              COMMAND                  CREATED       STATUS       PORTS                                         NAMES
# 2240735d4709   phpmyadmin:latest  "/docker-entrypoint.…"   5 hours ago   Up 5 hours   0.0.0.0:8080->80/tcp, [::]:8080->80/tcp       phpmyadmin
# 084713bdc9e4   mariadb:jammy      "docker-entrypoint.s…"   6 hours ago   Up 6 hours   0.0.0.0:3306->3306/tcp, [::]:3306->3306/tcp   world-db

# Example: Viewing the logs of a container
docker container logs 084

# Example: Inspecting a container
docker container inspect 084

# Example: Stopping a running container
docker container stop 084

# Example: Removing a stopped container
docker container rm 084
```
----------------

## Volume
Docker volumes are a way to persist data generated by and used by Docker containers. They provide a mechanism for storing data outside of the container's writable layer, allowing data to persist even when the container is deleted or recreated. Volumes are managed by Docker and can be easily shared between multiple containers.

### Common Docker Volume Commands
- `docker volume create <volume_name>`: Create a new Docker volume.
- `docker volume ls`: List all Docker volumes on the local machine.
- `docker volume rm <volume_name>`: Remove a Docker volume.
- `docker volume inspect <volume_name>`: Display detailed information about a Docker volume.
```bash
# Example: Creating a new Docker volume
docker volume create books-db
# books-db is the name of the created volume.

# Example: Listing all Docker volumes
docker volume ls
# DRIVER    VOLUME NAME
# local     books-db

# Example: Inspecting a Docker volume
docker volume inspect books-db
# [
#     {
#         "CreatedAt": "2023-10-01
#         "Driver": "local",
#         "Labels": {},
#         "Mountpoint": "/var/lib/docker/volumes/books-db/_data",
#         "Name": "books-db",
#         "Options": {},
#         "Scope": "local"
#     }
# ]

# Example: Removing a Docker volume
docker volume rm books-db
```
----------------

## Network
[network Documentation](https://docs.docker.com/engine/network/tutorials/standalone/)
