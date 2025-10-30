## Section 4

## Conectando a la antiguita: Postgres y pgAdmin

```bash
# 1. Crear red personalizada
docker network create postgres-net

# 2. Crear volumen para la base de datos Postgres
docker container run \
-dp 5432:5432 \
--name postgres-db \
-e POSTGRES_PASSWORD=123456 \
-v postgres-db:/var/lib/postgresql/data \
postgres:15.1


# 3. Levantar pgAdmin
docker container run \
--name pgAdmin \
-e PGADMIN_DEFAULT_PASSWORD=123456 \
-e PGADMIN_DEFAULT_EMAIL=superman@google.com \
-dp 8080:80 \
dpage/pgadmin4:6.17

# 4. conectarlos a la red personalizada
docker network connect postgres-net postgres-db
docker network connect postgres-net pgAdmin

# 5. Acceder a pgAdmin desde el navegador
# URL: http://localhost:8080
# 6. (Opcional) Detener y eliminar contenedores
docker container stop pgAdmin postgres-db
docker container rm pgAdmin postgres-db

# 7. (Opcional) Eliminar volumen y red
docker volume rm postgres-db
docker network rm postgres-net  
```

## Docker Compose
> Dockerfile, Compose, DockerHub, Registros, Despliegues, Volúmenes, Multi-staged builds, Multi architecture builds, y más
```yaml

```