#  PROYECTO SERVIDOR WEB 

### 1. Instalación del servidor web apache. Usaremos dos dominios mediante el archivo hosts: centro.intranet y departamentos.centro.intranet. El primero servirá el contenido mediante wordpress y el segundo una aplicación en Python

### **1.2. Instalación Servidor Web Apache**

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

### **1.3. Instalación de MySql y PHP**
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

### **Configurar Dominios**
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

### Instalacion y Configuración de Worpress
Como ya tenemos instalado PHP, Apache y MySQL pasamos a crear nuestra base de datos para wordpress
Acedemos a MySQL
```bash
sudo mysql -u root -p

```
Y ecribimos el siguiente codigo
```sql
CREATE DATABASE wordpress;
CREATE USER 'nombre_usuario'@'localhost' IDENTIFIED BY 'tu_contraseña';
GRANT ALL PRIVILEGES ON wordpress.* TO 'nombre_usuario'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```


Para instalar Wordpress,nos dirigirnos a **/var/www/html/** y creamos un directorio donde se va almacenar nuestra página de Wordpress

```bash
sudo mkdir /var/www/html/pagWordPress
```
