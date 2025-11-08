```bash
# 1. Creamos la red personalizada
docker network create world-app

# 2. Crear volumen ("el disco") para la base de datos
docker volume create world-db

# 3. Descargar las imágenes necesarias
docker container pull mariadb:jammy
docker container pull phpmyadmin:5.2.0-apache

# 4. Levantar DB
docker container run \
-dp 3306:3306 \
--name world-db \
--env MARIADB_USER=test-user \
--env MARIADB_PASSWORD=testPass \
--env MARIADB_ROOT_PASSWORD=rootPass \
--env MARIADB_DATABASE=world-db \
--volume world-db:/var/lib/mysql \
--network world-app \
mariadb:jammy

# 5. Levantar phpMyAdmin
docker container run \
--name phpmyadmin \
-d \
-e PMA_ARBITRARY=1 \
-p 8080:80 \
--network world-app \
phpmyadmin:5.2.0-apache

# 6. Acceder a phpMyAdmin desde el navegador
# URL: http://localhost:8080

# 7. (Opcional) Detener y eliminar contenedores
docker container stop phpmyadmin world-db
docker container rm phpmyadmin world-db

# 8. (Opcional) Eliminar volumen y red
docker volume rm world-db
docker network rm world-app

```

## Bind Volumes

```bash
docker pull node:16.16-alpine3.15

# correr la app de nest
docker container run \
--name nest-app \
-w /app \
-p 3000:3000 \
-v ${PWD}:/app \
--user $(id -u):$(id -g) \
node:18-alpine3.16\
sh -c "yarn install && yarn start:dev"
```

## Notas y errores

tuve muchos problemas con los contextos y los permisos de usuarios que no se buindeaban bien, 
al final con `--user $(id -u):$(id -g)` no lo logre y tanto chat gpt como claude o gemini me daban mas o menos las mismas alternativas.
### Problemas:
el problema es que el comando para correr la app de nest es solo funcionaba cuando lo corría con sudo `sudo docker container run \... etc` pero al correrlo asi la GUI de Docker-desktop no lo ve.

### Casi solución:
aunque no logre que la GUI de docker-desktop lo vea, logre correr la app sin sudo, cambiando el contexto de docker a "default" en lugar de "desktop-linux", con eso pude correr la app sin sudo, pero no se veía en la GUI de docker-desktop.
```bash
docker context ls               
# NAME              DESCRIPTION                               DOCKER ENDPOINT                                 ERROR
# default           Current DOCKER_HOST based configuration   unix:///var/run/docker.sock                     
# desktop-linux *   Docker Desktop                            unix:///home/user/.docker/desktop/docker.sock   

docker context use default
```

### Recursos
- https://stackoverflow.com/questions/51440656/docker-volume-permission-denied
- https://docs.docker.com/engine/reference/commandline/container_run/#set-the-user
- https://docs.docker.com/engine/reference/commandline/context_ls/
- https://docs.docker.com/engine/reference/commandline/context_use/ 
