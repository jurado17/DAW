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
sudo apt install mysql-server
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
cd /etc/hosts
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
CREATE DATABASE wordpress;
CREATE USER 'luis'@'localhost' IDENTIFIED BY '12345';
GRANT ALL PRIVILEGES ON wordpress.* TO 'luis'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```
Ahora pasamos a descargar la ultima version de wordpress desde la pagina oficial
```bash
wget https://wordpress.org/latest.tar.gz
```
Y descomprimimos el archivo
```bash
tar -zxvf latest.tar.gz
```
Y movemos la carpeta de Wordpress a la carpeta del servidor
```bash
sudo mv wordpress /var/www/html/
```
### 2.2. Configurar Wordpress
Para configurar wordpress vamos adirigirnos a la carpeta de wordpress, copiamos el archivo de configuración y lo editamos

```bash
cd /var/www/html/wordpress
sudo cp wp-config-sample.php wp-config.php
sudo nano wp-config.php
```
Una vez dentro actualizamos las lineas de informacion de la base de datos con la informacion que configuramos anteriormente
```php
define('DB_NAME', 'wordpress');
define('DB_USER', 'luis');
define('DB_PASSWORD', '12345');
define('DB_HOST', 'localhost');
```
### 2.3. Permisos
Para terminar la configuracion ajustamos los permisos de los archivos y las carpetas y reiniciamos apache
```bash
sudo chown -R www-data:www-data /var/www/html/wordpress
sudo chmod -R 755 /var/www/html/wordpress
sudo systemctl restart apache2
```
### 2.4. Configuración de WordPress en Apache para centro.intranet y departamentos.centro.intranet
#### 1.5.1 Centro.intranet
Habiendo ya instalado Wordpress, creamos un nuevo archivo de configuracion para 'centro.intranet'
```bash
sudo nano /etc/apache2/sites-available/centro.intranet.conf
```
Y añadimos las siguientes lineas a la configuracion
```apache
<VirtualHost *:80>
    ServerAdmin webmaster@centro.intranet
    ServerName centro.intranet
    DocumentRoot /var/www/centro.intranet/public_html

    <Directory /var/www/centro.intranet/public_html>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/centro.intranet_error.log
    CustomLog ${APACHE_LOG_DIR}/centro.intranet_access.log combined
</VirtualHost>
```
Guardamos y habilitamos el sitio y reiniciamos apache para aplicar los cambios.
```bash
sudo a2ensite centro.intranet.conf
sudo systemctl restart apache2
```
### 2.5. Departamnetos.centro.intranet
Para configurar departamentos.centro.intranet tendremos que realizar los mismos pasos de antes.

```bash
sudo nano /etc/apache2/sites-available/departamentos.centro.intranet.conf
```
Y añadimos las siguientes lineas a la configuracion
```apache
<VirtualHost *:80>
    ServerAdmin webmaster@departamentos.centro.intranet
    ServerName departamentos.centro.intranet
    DocumentRoot /var/www/departamentos.centro.intranet/public_html

    <Directory /var/www/departamentos.centro.intranet/public_html>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/departamentos.centro.intranet_error.log
    CustomLog ${APACHE_LOG_DIR}/departamentos.centro.intranet_access.log combined
</VirtualHost>
```
Guardamos y habilitamos el sitio y reiniciamos apache para aplicar los cambios.
```bash
sudo a2ensite departamentos.centro.intranet.conf
sudo systemctl restart apache2
```
# 3. Activar el modulo 'wsgi'
