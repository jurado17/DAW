#  PROYECTO SERVIDOR WEB 

### 1. Instalación del servidor web apache. Usaremos dos dominios mediante el archivo hosts: centro.intranet y departamentos.centro.intranet. El primero servirá el contenido mediante wordpress y el segundo una aplicación en Python

### **1.1. Instalación Servidor Web Apache**

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

### **1.2. Instalación de MySql y PHP**
Ahora deberemos instalar MySql con el siguiente comando
```bash
sudo apt install mysql-server -y
```

Una vez instalado, para saber si lo hicimos correctamente ponemos en la linea de comandos la siguiente instrucción
```bash
mysql
```
A continuación, vamos a pasar a instalar PHP, para ellos ejecutamos el sigueinte comando para instalar algunas librerias necesarias
```bash
sudo apt install php libapache2-mod-php php-mysql php-curl php-gd php-mbstring php-xml php-xmlrpc php-soap php-intl php-zip

```
Comprobamos que se ha instalado correctamente viendo la version de PHP que hemos instalado
```bash
php -v
```

### **1.3. Configurar Dominios**
Ahora crearemos los dominio y para ello tenemos que dirigirnos al fichero **/etc/hosts**, e introducimos el sigiuente comando:

```bash
sudo nano /etc/hosts
```
Una vez dentro añadimos las siguientes líneas
```
127.0.0.1  centro.intranet
127.0.0.1  departamentos.centro.intranet
```

# 2. Instalacion y Configuración de Worpress
### 2.1 Instalar Wordpress
Como ya tenemos instalado PHP, Apache y MySQL pasamos a crear nuestra base de datos para wordpress
Acedemos a MySQL
```bash
sudo mysql -u root -p

```
Y ecribimos el siguiente codigo
```sql
sudo mysql
mysql> CREATE DATABASE wordpressdb;
mysql> CREATE USER 'luis'@'localhost' IDENTIFIED BY '12345';
mysql> GRANT ALL PRIVILEGES ON wordpressdb.* TO 'luis'@'localhost' WITH GRANT OPTION;
mysql> FLUSH PRIVILEGES;
EXIT;
```
Creamos una carpeta en /var/www/html llamada centro.intranet
```shell
cd /var/www/html
sudo mkdir centro.intranet
```
Ahora pasamos a descargar la ultima version de wordpress desde la pagina oficial y descomprimimos el archivo
```bash
sudo wget -c https://wordpress.org/latest.tar.gz
sudo tar -zxvf latest.tar.gz
```
Cambiamos los permisos de la carpeta y reiniciamos apache
```shell
sudo chown -R www-data:www-data centro.intranet
sudo chmod -R 755 centro.intranet
sudo systemctl restart apache2
```
### 2.2. Configuración de WordPress en Apache para centro.intranet 
Habiendo ya instalado Wordpress, creamos un nuevo archivo de configuracion para 'centro.intranet'
```bash
sudo nano /etc/apache2/sites-available/centro.intranet.conf
```
Y añadimos las siguientes lineas a la configuracion
```apache
<VirtualHost *:80>
    ServerAdmin luis@localhost
    ServerName centro.intranet
    DocumentRoot /var/www/centro.intranet

    <Directory /var/www/centro.intranet>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
   CustomLog ${APACHE_LOG_DIR}/access.log comnined
</VirtualHost>
```
Le agregamos la configuracion de ServerName al archivo apache2.conf. Comprobamos que si estan bien la configuracion.
```shell
sudo nano apache2.conf

ServerName localhost

apachectl -t
```
Guardamos y habilitamos el sitio y reiniciamos apache para aplicar los cambios.
```bash
sudo a2ensite centro.intranet.conf
sudo systemctl reload apache2

sudo a2dissite 000-default.conf
sudo systemctl reload apache2
```

# 3. Activar el modulo 'wsgi'
Creamos una carpeta donde estara nuestro archivo de python. esta estara en var/www/html, se llamara departamentos.centro.intranet creamos la carpeta y le daremos los permisos necesarios.

```shell
cd /var/www/html

mkdir departamentos.centro.intranet
nano app.py 
```
Guardamos y ahora le daremos los permisos a la carpeta

```shell
sudo chown -R www-data:www-data departamentos.centro.intranet
sudo chmod -R 755 departamentos.centro.intranet
```
En etc/apache2/sites-available creamos en .conf, y pondremos lla pagina en los sitios actiados

```shell
sudo nano departamentos.centro.intranet.conf

<VirtualHost *:80>
    ServerName departamentos.centro.intranet
    DocumentRoot /var/www/html/departamentos.centro.intranet
    WSGIScriptAlias / /var/www/html/departamentos.centro.intranet/app.py

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Guardamos y habilitamos el sitio y reiniciamos apache para aplicar los cambios.
```
sudo a2ensite departamentos.centro.intranet.conf 
sudo systemctl reload apache2
```
## 4. Crea y despliega una pequeña aplicación python para comprobar que funciona correctamente.

Creamos un archivo .py
```python
def application(environ, start_response):
    status = '200 OK'
    output = b'Hello, World!'

    response_headers = [('Content-type', 'text/plain'), ('Content-Length', str(len(output)))]
    start_response(status, response_headers)

    return [output]
```

## 5. Adicionalmente protegeremos el acceso a la aplicación python mediante autenticación

A nuestro archivo .conf le agregamos los siguiente:
```shell
<Directory /var/www/html/departamentos.centro.intranet>
    AuthType Basic
    AuthName "Restricted Access"
    AuthUserFile /var/www/html/departamentos.centro.intranet/.htpasswd
    Require valid-user
</Directory>
```
En la carpeta donde guardamos la aplicacion (departamentos.centro.intranet) creamos una archivo .htpasswd y le añadimos los permisos necesarios 

```shell
sudo htpasswd -c /var/www/html/departamentos.centro.intranet/.htpasswd usuario

sudo chmod 644 /var/www/html/departamentos.centro.intranet/.htpasswd
sudo chown www-data:www-data /var/www/html/departamentos.centro.intranet/.htpasswd

sudo systemctl restart apache2
```
Comprobamos en la pagina

## 6. Instalamos awstat

Instalamos awstat

```shell
sudo apt-get update
sudo apt-get install awstats
```
Para editarlo

```shell
sudo nano /etc/awstats/awstats.conf
```
## 7. Configuración del segundo servidor con Nginx:

Instalar Nginx y PHP
```shell
sudo apt-get install nginx php-fpm
```
Instala phpMyAdmin
```shell
sudo apt-get install phpmyadmin
```

Configura phpMyAdmin para Nginx
```shell
sudo nano /etc/nginx/sites-available/phpmyadmin

server {
    listen 8080;
    server_name phpmyadmin.centro.intranet;

    root /usr/share/phpmyadmin;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.ht {
        deny all;
    }
}
```
Guardamos y reiniciamos 
```shell
sudo ln -s /etc/nginx/sites-available/phpmyadmin /etc/nginx/sites-enabled/
sudo systemctl restart nginx
```

Agregamos los dominios en etc/hosts

```shell
sudo nano /etc/hosts

127.0.0.1 servidor2.centro.intranet
127.0.0.1 phpmyadmin.centro.intranet
```

