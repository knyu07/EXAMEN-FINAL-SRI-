# EXAMEN DE REDES - FINAL

## Instalación de la pila LAMP

Actualizamos repositorios 

- sudo apt update

Instalamos apache

- sudo apt install apache2 -y 

Instalamos MySQL

- sudo apt install mysql-server -y 

Actualizamos la contraseña de root de MySQL (entramos dentro de mysql)

- ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'root';

- FLUSH PRIVILEGES;

Instalamos los módulos de PHP

- sudo apt install php libapache2-mod-php php-mysql -y

## Instalación de Wordpress

Añadimos la URL del Wordpress

- cd /var/www/html
- wget http://wordpress.org/latest.tar.gz

Descomprimimos el .tar.gz

- tar -xzvf latest.tar.gz

Eliminamos el tar.gz

- rm latest.tar.gz

Creamos la base de datos para wordpress

- DROP DATABASE IF EXISTS db_wordpress;
- CREATE DATABASE db_wordpress;
- CREATE USER db_user@localhost IDENTIFIED BY 'db_password';
- GRANT ALL PRIVILEGES ON db_wordpress.* TO db_user@localhost;
- FLUSH PRIVILEGES;

Configuramos  el archivo de configuración de Wordpress

- cd /var/www/html/wordpress
- mv wp-config-sample.php wp-config.php

- sed -i "s/database_name_here/db_wordpress/" wp-config.php
- sed -i "s/username_here/db_user/" wp-config.php
- sed -i "s/password_here/db_password/" wp-config.php

Habilitamos las variables WP_SITEURL y WP_HOME

sed -i "/DB_COLLATE/a define('WP_SITEURL', 'http://$IP_MAQUINA/wordpress');" /var/www/html/wordpress/wp-config.php
sed -i "/WP_SITEURL/a define('WP_HOME', 'http://$IP_MAQUINA');" /var/www/html/wordpress/wp-config.php

## CERBOT

Instala core desde snap 

- sudo snap install core; sudo snap refresh core

Instalamos Cerbot

- sudo snap install --classic certbot

Reconocemos el comando Certbot

- sudo ln -s /snap/bin/certbot /usr/bin/certbot

Elegimos servicio que correrá cerbot

- certbot --apache -m demo@demo.es --agree-tos -d nombre_dominio

Cerfiticamos 

- sudo certbot certonly --apache

## Activar Mod_rewrite  para los enlaces amigables de wordpress.

- sudo a2enmod rewrite

## #Movemos el archivo htaccess a /var/www/html

- sudo touch /var/www/html/.htaccess 

Escribimos contenido: 

```
# BEGIN WordPress
<IfModule mod_rewrite.c>
RewriteEngine On
RewriteBase /
RewriteRule ^index\.php$ - [L]
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule . /index.php [L]
</IfModule>
# END WordPress
```

