# Tarea 3


## 1. Utiliza la imagen de Ubuntu , tag 22 y apoyandote en **esta guía** sigue sus instrucciones para instalar **LAMP** en dicho contenedor.
```bash
docker pull ubuntu:22.04
docker images
```
![ubuntupull](https://github.com/user-attachments/assets/c37f6201-a958-4f0c-b847-e03b6c460b5c)

```bash
docker container create -i -t --name cont_ubu1 ubuntu:22.04
```
### Iniciar parar container: docker container start cont_ubu1


### Instalar LAMP en dicho contenedor:

#### Instalar Apache
```bash
sudo apt update
sudo apt install -y apache2 apache2-utils
```
Comprobamos que apache se instaló correctamente:
```bash
sudo systemctl status apache2
```
![apache instalado](https://github.com/user-attachments/assets/f12d9f05-9b04-4aee-a178-ad3b7c5a502f)



#### Instalar MariaDB
```bash
sudo apt install -y mariadb-server mariadb-client
sudo mysql_secure_installation
```
Tras configurar los valores nos aparece el mensaje: Thanks for using MariaDB!
```bash
sudo systemctl status mariadb
```
![mariaDB instalado](https://github.com/user-attachments/assets/848741f8-9a03-409e-890b-a9da5164556a)



#### Instalar PHP
```bash
sudo apt install -y php php-mysql libapache2-mod-php
sudo systemctl restart apache2
```
Comprobamos que php este instalado:
```bash
php -v
```
![phpinstalado](https://github.com/user-attachments/assets/626c2480-d61f-4f2b-8755-31e0e9289398)



## 2. Utiliza **esta guía** para instalar wordpress en el contenedor.
```bash

```
## 3. Comprueba que puedes acceder a wordpress. 
```bash

```
## OPCIONAL: Instala phpmyadmin en el contenedor siguiendo esta guía. Comprueba que puedes acceder.
```bash

```
