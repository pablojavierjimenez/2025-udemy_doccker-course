# Seccion 2 - Bases de Docker
También empezaremos con nuestros primeros comandos:

- docker container:
    - run
    - remove
    - list
    - publish
    - environment variables
    - logs
    - detached
    - docker pull

## S2 - 12
```bash
# Descargar una imagen
$ sudo docker pull hello-world

# Correr un contenedor
$ sudo docker container run hello-world
```
--------------

## S2-13
```bash
# el comando ls-a lista solo los contenedores activos
:~$ sudo docker container ls

# el comando 'ls -a' o 'ls --all' lista todos los contenedores activos o detenidos
:~$ sudo docker container ls -a

CONTAINER ID   IMAGE         COMMAND    CREATED          STATUS                      PORTS     NAMES
234d22532cdf   hello-world   "/hello"   4 minutes ago    Exited (0) 4 minutes ago              quizzical_shirley
16b736bbfbe2   hello-world   "/hello"   4 minutes ago    Exited (0) 4 minutes ago              busy_ramanujan
f58c34b70a65   hello-world   "/hello"   17 minutes ago   Exited (0) 17 minutes ago             agitated_compton
355b6bb34a05   hello-world   "/hello"   20 minutes ago   Exited (0) 20 minutes ago             agitated_
9c2f0be09612   hello-world   "/hello"   7 weeks ago      Exited (0) 7 weeks ago                trusting_yalow

# Detener un contenedor especifico
#   el comando rm puede detener 1 o varios simultaneamente,
#   utilizando el container ID el nombre, o solo los 3 primeros caracteres de cada container id
:~$ sudo docker container rm 234d22532cdf
:~$ sudo docker container rm 16b f58

# prune eliminara todos los contenedores que esten detenidos
:~$ sudo docker container prune

```
lo mismo para las imagenes
```bash
# comentario
:~$ sudo docker image ls
:~$ sudo docker image rm hello-world

```
--------------

## S2-16 - Variables de entorno
```bash
# Instalamos postgres, para luego hacer pruebas
:~$ sudo docker run --name test-postgres -e POSTGRES_PASSWORD=testpassword -d postgres

:~$ sudo docker 
:~$ sudo docker 
:~$ sudo docker 

```
--------------

## S2-1
```bash
# comentario
:~$ sudo docker 

```
--------------
