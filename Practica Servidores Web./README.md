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
