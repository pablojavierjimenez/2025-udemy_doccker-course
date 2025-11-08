## Section 4

### Links útiles

- [Tarea sobre PGAdmin y Postgres](https://gist.github.com/Klerith/8cfc637868212cfb888333ecaa6080e1)
- [Postgres: postgres:15.1](https://hub.docker.com/_/postgres)
- [PgAdmin: dpage/pgadmin4:6.17](https://hub.docker.com/r/dpage/pgadmin4)
- [Primer ejemplo de docker-compose postgres-pgadmin](https://gist.github.com/Klerith/86baf403cc922d03c86fac0b5ceacd3b)
- [PG Admin - Docs](https://www.pgadmin.org/docs/pgadmin4/latest/container_deployment.html#mapped-files-and-directories)

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

### NOTA:

_**para no tener problemas con los permisos y en que contexto corre etc etc.
el problema esta en que docker-desktop no tiene acceso a los discos por fuera del file system nativo, como los montados con NTFS o fat.
asi que la veintiunica solución real es mover las carpetas a el disco del file system nativo (ext4 en linux)**_

> Dockerfile, Compose, DockerHub, Registros, Despliegues, Volúmenes, Multi-staged builds, Multi architecture builds, y más

> `sudo chown -R5050:5050 pgadmin`

### Comandos para correr el docker

```bash
# 1. Levantar servicios
docker compose up -d
# 2. Acceder a pgAdmin desde el navegador
# URL: http://localhost:8080
# 3. Detener y eliminar servicios
docker compose down
```

---

### Docker Compose File para PostgressSQL - PGAdmin

- [40. Bind Volumes - Docker Compose (Video Tutorial)](https://www.udemy.com/course/docker-guia-practica/learn/lecture/35204992#overview)
- [PostgresSQL - Documentation](https://www.postgresql.org/docs/)
- [PGAdmin - Documentation](https://www.pgadmin.org/docs/)

### Steps:

1. ir a **dockerHub** y buscar las versiones que usaremos:
   - [PostgresSQL on dockerHub](https://hub.docker.com/_/postgres)
   - [PGAdmin on dockerHub](https://hub.docker.com/r/dpage/pgadmin4)
2. buscar en el overview de la DB (en este caso mongo) donde es que se persiste la data en el volumen,
   buscar palabras claves como "volumen", "Store Data", "stor" etc.

```yaml
# version: '3.8'
services:
  db:
    container_name: postgres-db
    image: postgres:15.1
    ports:
      - "5432:5432"
    volumes:
      # - postgres-db:/var/lib/postgresql/data # 👈 (usando volumen docker)
      # * ╔══ esta es la forma usando un volumen enlazado a una carpeta en la maquina host
      # * 👇  mas a bajo al final se aclara que "postgres-db" es externo
      - ./postgres:/var/lib/postgresql/data # (usando volumen bind mount, osea creando una carpeta en el host)
    environment:
      - POSTGRES_PASSWORD=123456

  pgadmin:
    container_name: pgadmin
    image: dpage/pgadmin4:6.17
    ports:
      - "8080:80"
    volumes:
      # * 👇 esta es igual que la DB apunta a una carpeta en la maquina host
      - ./pgadmin:/var/lib/pgadmin # 👈 (usando volumen bind mount, osea creando una carpeta en el host)
    environment:
      - PGADMIN_DEFAULT_PASSWORD=123456
      - PGADMIN_DEFAULT_EMAIL=superman@google.com

volumes:
  postgres-db:
    external: true
```

---

## Docker Compose ( parte 2 ) MongoDB - MongoExpress

- [Pokemon-app - docker-compose.yml](https://www.udemy.com/course/docker-guia-practica/learn/lecture/35217966#overview)
- [MongoDB - Documentation](https://www.mongodb.com/docs/get-started/)
- [Mongo-Express - Documentation](https://github.com/mongo-express/mongo-express?tab=readme-ov-file#mongo-express)

### Steps:

1. ir a **dockerHub** y buscar las versiones que usaremos:
   - [MongoDB on dockerHub](https://hub.docker.com/_/mongo)
   - [MongoExpress (Web-based MongoDB admin interface)](https://hub.docker.com/_/mongo-express)
2. buscar en el overview de la DB (en este caso mongo) donde es que se persiste la data en el volumen,
   buscar palabras claves como "volumen", "Store Data", "stor" etc.

### Comandos para correr el docker-compose.yml

```bash
# 1. Levantar servicios con menos d (-d) es para hacer detach de la terminal
#    Si lo corro solo con `docker compose up` la terminal seguirá capturada
#    aunque esto es bueno si queremos ver los logs
docker compose up -d
# 2. Acceder a pgAdmin desde el navegador
# URL: http://localhost:8080
# 3. Detener y eliminar servicios
docker compose down
```

---

### Docker Compose File para MongoDB - MongoExpress

**NOTA:**

> Recordar que siempre que se corre un docker compose, docker automáticamente crea una red 'virtual'
> para que los distintos contenedores se puedan comunicar entre ellos.
> entonces por ejemplo nosotros podríamos no exponer los puertos que corren dentro de esa red
> a nuestra maquina host. y aunque el host no pueda acceder al servicio corriendo en ese puerto dentro del contenedor, los programas corriendo en los otros contenedores, dentro del docker compose si tendrán acceso al servicio corriendo dentro de la red.

**_En este ejemplo eliminamos la exposición del puerto de la DB de Mongo, hacia la maquina host,
pero la app de mongo express que corre dentro de la misma red aun tiene acceso._**

```yaml
# ----------------------------------------------------------
# * Las variables de entorno las trae de el archivo ( .env )
# ----------------------------------------------------------
services:
  db:
    container_name: ${MONGO_DB_NAME}
    image: mongo:6.0
    volumes:
      - poke-vol:/data/db
    # ports:
    #  - "27017:27017" # 👈 asi  no expone su servicio a la maquina host
    restart: always
    environment:
      MONGO_INITDB_ROOT_USERNAME: ${MONGO_USERNAME}
      MONGO_INITDB_ROOT_PASSWORD: ${MONGO_PASSWORD}
    command: ["--auth"]

  mongo-express:
    depends_on:
      - db
    container_name: mongo-express
    image: mongo-express:1.0.0-alpha.4
    ports:
      - "8081:8081"
    restart: always
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: ${MONGO_USERNAME}
      ME_CONFIG_MONGODB_ADMINPASSWORD: ${MONGO_PASSWORD}
      ME_CONFIG_MONGODB_SERVER: ${MONGO_DB_NAME} # ! 👈 aquí falla si uso el nombre o la variable de la DB
      # ME_CONFIG_BASICAUTH: false   # opcional para pruebas locales

  poke-app_backend:
    depends_on:
      - db
      - mongo-express
    container_name: "poke-app_backend"
    image: klerith/pokemon-nest-app:1.0.0
    ports:
      - "3000:3000"
    environment:
      MONGODB: mongodb://${MONGO_USERNAME}:${MONGO_PASSWORD}@${MONGO_DB_NAME}:27017
      DB_NAME: ${MONGO_DB_NAME}
    restart: always
volumes:
  poke-vol:
    external: false
```

**Nota:**
En un momento no me funcionaba ME_CONFIG_MONGODB_SERVER y en DB_NAME tube que usar **"db"** que es el nombre del servicio,
porque con el nombre de la db no funciona no importa si llega<br/>
como variable -> ME_CONFIG_MONGODB_SERVER: ${MONGO_DB_NAME}<br/>
o directa -> ME_CONFIG_MONGODB_SERVER: pokemon_db

**Dot Env File**

```bash
MONGO_DB_NAME=pokemon_db
MONGO_USERNAME=testuser
MONGO_PASSWORD=123456
```
