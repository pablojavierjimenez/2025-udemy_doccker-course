# Docker Terminal Command

## Container Management Commands
- `docker ps`: Lists all running containers.
- `docker ps -a`: Lists all containers, including stopped ones.
- `docker start <container_id>`: Starts a stopped container.
- `docker stop <container_id>`: Stops a running container.
- `docker restart <container_id>`: Restarts a running container.
- `docker rm <container_id>`: Removes a stopped container.
- `docker logs <container_id>`: Fetches the logs of a container.

```bash
docker container pull docker/getting-started
docker container run docker/getting-started -d --name getting-started
docker ps          
docker container ls
docker container ls --all
docker container logs 9d9
docker container stop 9d9
docker container start 9d9
docker container restart 9d9
docker container rm 9d9   
docker container rm -f 9d9   
docker container prune
docker container logs <container_id>
```
-----

## Image Management Commands
- `docker images`: Lists all Docker images on the local machine.
- `docker rmi <image_id>`: Removes a Docker image.
- `docker pull <image_name>`: Downloads a Docker image from a registry.
- `docker build -t <image_name> .`: Builds a Docker image from a Dockerfile in the current directory.
- `docker tag <source_image> <target_image>`: Tags an image with a new name.
- `docker push <image_name>`: Uploads a Docker image to a registry.

## Volume Management Commands
- `docker volume ls`: Lists all Docker volumes.
- `docker volume create <volume_name>`: Creates a new Docker volume.
- `docker volume rm <volume_name>`: Removes a Docker volume.
- `docker volume inspect <volume_name>`: Displays detailed information about a Docker volume.

## Network Management Commands
- `docker network ls`: Lists all Docker networks.
- `docker network create <network_name>`: Creates a new Docker network.
- `docker network rm <network_name>`: Removes a Docker network.
- `docker network inspect <network_name>`: Displays detailed information about a Docker network.
```bash
docker image ls
docker image rm <image_id>
docker image pull nginx:latest
docker image build -t my-nginx-image .
docker image tag my-nginx-image myrepo/my-nginx-image:latest
docker image push myrepo/my-nginx-image:latest
docker volume ls
docker volume create my-volume
docker volume rm my-volume
docker volume inspect my-volume
docker network ls
docker network create my-network
docker network rm my-network
docker network inspect my-network
```
-----
