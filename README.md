# Docker - Guía práctica de uso para desarrolladores

[explicado en este video - ](https://www.udemy.com/course/docker-guia-practica/learn/lecture/35171778#overview)
[instalaciones iniciales Prerequisitos](https://gist.github.com/Klerith/3f611ff0e5c15b733ac63365ab310a35)

## Seccion 3 volumenes y redes

1. Montar la imagen de MariaDB con el tag jammy, publicar en el puerto 3306 del contenedor con el puerto 3306 de nuestro equipo, colocarle el nombre al contenedor de **world-db** (--name world-db) y definir las siguientes variables de entorno:

   - MARIADB_USER=testUser
   - MARIADB_PASSWORD=testPass
   - MARIADB_ROOT_PASSWORD=root-secret-password
   - MARIADB_DATABASE=world-db

2. Conectarse usando Table Plus a la base de datos con las credenciales del usuario (NO EL ROOT)
3. Conectarse a la base de datos `world-db`
4. Ejecutar el query de creación de tablas e inserción proporcionado
5. Revisar que efectivamente tengamos la data

```bash
:~$ docker run \
    --name world-db \
    -e MARIADB_USER=testUser \
    -e MARIADB_PASSWORD=testPass \
    -e MARIADB_ROOT_PASSWORD=root-secret-password
    -e MARIADB_DATABASE=world-db \
    -dp 3306:3306 \
    -d mariadb:jammy
```

```sql
CREATE TABLE IF NOT EXISTS `cities` (
  `ID` int NOT NULL AUTO_INCREMENT,
  `Name` varchar(35) NOT NULL,
  `CountryCode` char(3) NOT NULL,
  `District` varchar(20) NOT NULL,
  `Population` int NOT NULL,
  PRIMARY KEY (`ID`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

---

## Volumen conectado con phpmyadmin

```bash
docker pull phpmyadmin:5.2.3-apache
docker container run \
--name phpmyadmin \
-d \
-e PMA_ARBITRARY=1 \
-p 8080:80 \
phpmyadmin:5.2.0-apache
```
