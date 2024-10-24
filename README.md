# Tarea 4
La siguiente práctica es una lista de tareas que tenéis que hacer. 

Crear un  fichero (nombre "Readme.md") con formato markdown en un repositorio en github para subir este fichero y hacer commits que indiquen lo realizado.

En la respuesta pon el enlace a tu repositorio en github

1. Utiliza la imagen de Ubuntu , tag 22 y apoyandote en esta [guía](https://www.itzgeek.com/how-tos/linux/ubuntu-how-tos/install-lamp-stack-apache-mariadb-php-on-ubuntu-22-04.html#Method_1_Install_LAMP_Stack_Manually_One_by_one) sigue sus instrucciones para instalar LAMP en dicho contenedor.

2. Utiliza esta [guía](https://ubuntu.com/tutorials/install-and-configure-wordpress#1-overview) para instalar wordpress en el contenedor.

3. Comprueba que puedes acceder a wordpress. 


OPCIONAL: Instala phpmyadmin en el contenedor siguiendo esta [guía](https://ubuntu.com/server/docs/how-to-install-and-configure-phpmyadmin). Comprueba que puedes acceder.


## 1. Utiliza la imagen de Ubuntu , tag 22 y apoyandote en **esta guía** sigue sus instrucciones para instalar **LAMP** en dicho contenedor.
```bash
docker pull ubuntu:22.04
docker images
```
![ubuntupull](https://github.com/user-attachments/assets/c37f6201-a958-4f0c-b847-e03b6c460b5c)

```bash
docker container create -i -t --name cont_ubu1 -p  8080:80 ubuntu:22.04
```
// paramos el contenedor y luego lo iniciamos con attack para que nos abra la terminal y seguimos la guia
// seguimos la guia con los comandos posterior
Iniciamos y accedemos al contenedor con attack:
```bash
docker container start --attach -i cont_ubu1
```

### Instalar LAMP en dicho contenedor:

#### Instalar Apache
```bash
apt update
apt install -y apache2 apache2-utils
```
Comprobamos que apache se instaló correctamente:
```bash
service apache2 start
service apache2 status
```
![apache_en_contenedor](https://github.com/user-attachments/assets/a7f1f95e-1a06-4841-9250-c9f51f71ae3e)




#### Instalar MariaDB
```bash
apt install -y mariadb-server mariadb-client
```
Una vez instalado tenemos que iniciar el servicio para configurar la seguridad.
```bash
service mariabd status
service mariabd start
mysql_secure_installation
```
Tras configurar los valores nos aparece el mensaje: Thanks for using MariaDB!

![mariadb_en_contenedor](https://github.com/user-attachments/assets/5771fcbb-a68d-48e3-b050-ad222e0d9e8b)



#### Crear base de datos en contenedor mariadb(para luego)
service mariadb status
service mariadb start
accede a mariadb(pedira la password de root(en mi caso 'admin'))

mysql -u root -p
create database mi_base_de_datos;
show databases;

![mi_base_de_datos](https://github.com/user-attachments/assets/06ac3b96-195a-4b93-b6d5-991e0d4b1bb1)



#### Instalar PHP
```bash
apt install -y php php-mysql libapache2-mod-php
```
Comprobamos que php este instalado:
```bash
php -v
```
![php_en_contenedor](https://github.com/user-attachments/assets/e53c4583-d855-4b78-a11f-52798ff4677a)

#### Test LAMP Stack
```bash
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
```
Abrimos el navegador y accedemos a nuestra web
![test_lamp](https://github.com/user-attachments/assets/e1ba5c02-d835-4f8f-99ef-7ccd260c758b)





## 2. Utiliza **esta guía** para instalar wordpress en el contenedor.
Seguimos dentro del contenedor
Instalamos las dependencias necesarias, lanzamos el siguiente comando que instalara muchos paquetes:

```bash
apt install apache2 \
                 ghostscript \
                 libapache2-mod-php \
                 mysql-server \
                 php \
                 php-bcmath \
                 php-curl \
                 php-imagick \
                 php-intl \
                 php-json \
                 php-mbstring \
                 php-mysql \
                 php-xml \
                 php-zip
```
Instalar WordPress
Create the installation directory and download the file from WordPress.org:
Instalar curl:

```bash
mkdir -p /srv/www
chown www-data: /srv/www
curl https://wordpress.org/latest.tar.gz | tar zx -C /srv/www
```
Para el ultimo comando necesitas instalar curl:
```bash
apt-get install -y curl
```
curl localhost/info.php #comprobamos que funciona el info.php

4. Configure Apache for WordPress.


## 3. Comprueba que puedes acceder a wordpress. 
```bash

```
## OPCIONAL: Instala phpmyadmin en el contenedor siguiendo esta guía. Comprueba que puedes acceder.
```bash

```
