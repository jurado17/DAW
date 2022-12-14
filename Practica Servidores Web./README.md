#  PROYECTO SERVIDOR WEB 

### 1. Instalación del servidor web apache. Usaremos dos dominios mediante el archivo hosts: centro.intranet y departamentos.centro.intranet. El primero servirá el contenido mediante wordpress y el segundo una aplicación en Python

### **Instalación Servidor Web Apache**

Primero actualizamos el sistema usando el siguiente comando
```bash
sudo apt update || apt upgrade
```

Una vez hecho, instalamos el servidor web apache y comprobamos el estado del servidor
```bash
sudo apt install apache2 -y
```

```bash
systemctl status apache2
```

### **Instalación de MySql y PHP**
Ahora deberemos instalar MySql con el siguiente comando
```bash
sudo apt install mysql-server
```

Una vez instalado, para saber si lo hicimos correctamente ponemos en la linea de comandos la siguiente instrucción
```bash
mysql
```
A continuación, vamos a pasar a instalar PHP, para ellos ejecutamos el sigueinte comando para instalar algunas librerias necesarias
```bash
apt install php libapache2-mod-php php-mysql
```
Comprobamos que se ha instalado correctamente viendo la version de PHP que hemos instalado
```bash
php -v
```

### **Configurar Dominios**
Ahora crearemos los dominio y para ello tenemos que dirigirnos al fichero **/etc/hosts**, e introducimos el sigiuente comando:

```bash
cd /etc/hosts
sudo nano /etc/hosts
```
### Instalacion y Configuración de Worpress
Para instalar Wordpress,nos dirigirnos a **/var/www/html/** y creamos un directorio donde se va almacenar nuestra página de Wordpress

```bash
sudo mkdir /var/www/html/pagWordPress
```

El segundo paso que debemos seguir, es crear el fichero de configuracion de nuestra página Wordpress. Para ello, nos dirigimos a **/etc/apache2/sites-available/centro.intranet.conf**, en mi caso el nombre del fichero es el nombre de mi dominio + .conf

```bash
sudo nano /etc/apache2/sites-available/centro.intranet.conf
```

Y ponemos el siguiente contenido

```apache
<VirtualHost *:80>
  ServerName centro.intranet
  ServerAlias centro.intranet
  ServerAdmin webmaster@localhost
  DocumentRoot /var/www/html/pagWordPress
  
<Directory /var/www/html/padWordPress/>
  AllowOverride All
</Directory>

ErrorLog ${APACHE_LOG_DIR}/pagWordPress_error.log
CustomLog ${APACHE_LOG_DIR}/pagWordPress_access.log combined
</VirtualHost>
```


Una vez creado, debemos comprobar si no hay ningun error sintáctico en la configuración, por lo que habilitamos el nuevo modulo que hemos creado y reiniciamos apache

```bash
sudo a2ensite centro.intranet
```
```bash
sudo apache2ctl configtest
```
```bash
sudo systemctl restart apache2
```

A continuación, vamos a ejecutar algunos comandos en mysql para crear una base de datos y un usuario administrador para Wordpress. Para ello, entramos en mysql

```bash
mysql
```

Una vez dentro, vamos a crear la base de datos para Wordpress, en mi caso se llamará **wordpress**

```mysql
CREATE DATABASE wordpress DEFAULT CHARACTER SET utf8 COLLATE utf8_unicode_ci;
```

Creamos el usuario administrador, en este caso, se llamará **wordpressuser**

```mysql
CREATE USER 'wordpressuser'@'%' IDENTIFIED WITH mysql_native_password BY '123';
```
