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
service mariadb status
service mariadb start
mysql_secure_installation
```
Tras configurar los valores nos aparece el mensaje: Thanks for using MariaDB!

![mariadb_en_contenedor](https://github.com/user-attachments/assets/5771fcbb-a68d-48e3-b050-ad222e0d9e8b)

<!-- -----------------------------------------------------
Chuela crear base de datos

#### Crear base de datos en contenedor mariadb(para luego)

service mariadb status

service mariadb start

accede a mariadb(pedira la password de root(en mi caso 'admin'))


mysql -u root -p
create database mi_base_de_datos;
show databases;

![mi_base_de_datos](https://github.com/user-attachments/assets/06ac3b96-195a-4b93-b6d5-991e0d4b1bb1)
--------------------------------------------------------- -->


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

4. Configuramos Apache para WordPress.

Create Apache site for WordPress. Create

verifico ruta
```bash
echo /etc/apache2/sites-available/wordpress.conf
```

cómo crear un archivo de configuración para un sitio de WordPress en un servidor Apache
```bash
cat <<EOF > /etc/apache2/sites-available/wordpress.conf
<VirtualHost *:80>
    DocumentRoot /srv/www/wordpress
    <Directory /srv/www/wordpress>
        Options FollowSymLinks
        AllowOverride Limit Options FileInfo
        DirectoryIndex index.php
        Require all granted
    </Directory>
    <Directory /srv/www/wordpress/wp-content>
        Options FollowSymLinks
        Require all granted
    </Directory>
</VirtualHost>
EOF
```

Habilitar el sitio:
```bash
a2ensite wordpress
```
Expliación del comando:

**a2ensite**: Es un script de utilidad que habilita sitios en Apache. Cambia la configuración para que Apache sirva el sitio especificado.

**wordpress**: Es el nombre del archivo de configuración del sitio que has creado (en tu caso, /etc/apache2/sites-available/wordpress.conf). Este comando crea un enlace simbólico en /etc/apache2/sites-enabled/, activando así el sitio.


 Habilitar la reescritura de URL:
```bash
a2enmod rewrite
```
Explicación del comando:

**a2enmod**: Es un script de utilidad que habilita módulos en Apache.

**rewrite**: Es el módulo que permite reescribir URLs, lo que es fundamental para WordPress. Permite crear URLs más limpias y amigables para SEO, y es necesario para que algunas funcionalidades de WordPress funcionen correctamente.



Deshabilitar el sitio por defecto:
```bash
a2dissite 000-default
```
Explicación del comando:

**a2dissite**: Es un script de utilidad que deshabilita sitios en Apache.

**000-default**: Este es el sitio predeterminado que Apache crea al instalarlo. Generalmente muestra una página de "¡Funciona!" cuando se accede al servidor. Deshabilitarlo es útil cuando deseas que tu sitio (en este caso, WordPress) sea el único que se sirva.

![configuracion](https://github.com/user-attachments/assets/c62c2ea2-05e5-4bcd-aa39-8ff6255b0188)

Reiniciamos Apache:
```bash
service apache2 restart
```

Para configurar WordPress tenemos que crear la base de datos MySQL: 

(\h para menú de ayuda)
```bash
mysql -u root
mysql> CREATE DATABASE wordpress;
mysql> CREATE USER wordpress@localhost IDENTIFIED BY '<your-password>';
mysql> CREATE USER wordpress@localhost IDENTIFIED BY 'admin';
mysql> GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER ON wordpress.* TO wordpress@localhost;
mysql> FLUSH PRIVILEGES;
mysql> quit
```
Enable MySQL with service mysql start.

---------------------------------




## 3. Comprueba que puedes acceder a wordpress. 
Ahora ya podemos a acceder a WordPress con nuestra direccion en mi caso: **http://10.0.9.154:8080/wp-admin/setup-config.php**

![Screenshot_20241028_105945](https://github.com/user-attachments/assets/d5c16987-238a-4cd8-915d-fb29a076aa25)


## OPCIONAL: Instala phpmyadmin en el contenedor siguiendo esta guía. Comprueba que puedes acceder.
```bash
En este caso aún no me ha dado tiempo!
```
