# Práctica 2º Trimeste - Luis Jurado

## Práctica 1- Servidor alojamiento web

Se pide las instalación, configuración y puesta en marcha de un servidor que ofrezca servicio de alojamiento web configurable:

- Se dará **alojamiento a páginas web** tanto estáticas como dinámicas con “**php**”

- Los clientes dispondrán de un **directorio de usuario con una página web** por defecto.

- Además contarán con una base de datos **sql** que podrán administrar con **phpmyadmin**

- Los clientes podrán acceder mediante **ftp** para la administración de archivos configurando adecuadamente TLS

- Se habilitará el acceso mediante **ssh y sftp**.

- Se automatizará mediante el uso de **scripts**:

  - La creación de usuarios y del directorio correspondiente para el alojamiento web

  - Host virtual en apache

  - Creación de usuario del sistema para acceso a ftp, ssh, smtp, …
  - Se creará un subdominio en el servidor DNS con las resolución directa e inversa
  - Se creará una base de datos además de un usuario con todos los permisos sobre dicha base de datos (ALL PRIVILEGES)
  - Se habilitará la ejecución de aplicaciones Python con el servidor web

## Instalación de Apache para alojar páginas web

Primero actualizamos el sistema operativo y todos los paquetes:

```bash
sudo apt update && sudo apt upgrade
```

![](https://github.com/jurado17/DAW/blob/main/Parctica_2_Trimestre/img/c1.png)

Y luedo instalaremos Apache:

```bash
sudo apt install apache2
```

![](https://github.com/jurado17/DAW/blob/main/Parctica_2_Trimestre/img/c2.png)

Verificamos que se haya instalado correctamente con el siguiente comando

```bash
sudo systemctl status apache2
```

![](https://github.com/jurado17/DAW/blob/main/Parctica_2_Trimestre/img/c3.png)

En caso de tener un firewall habilitado, utilizaremos este comando para permitir a los clientes conectarse al servidor

```bash
sudo ufw allow 'Apache'
```

Puedes verificar que Apache esté funcionando correctamente abriendo un navegador web y navegando a http://localhost o http://127.0.0.1.

![](https://github.com/jurado17/DAW/blob/main/Parctica_2_Trimestre/img/c4.png)

## Instalación de PHP

Para instalar PHP primero deberemos asegurarnos de que todos este actualizado y en la ultima versión posible asi evitaremos que haya problemas por incompatibilidades. Esto lo realizaremos siempre antes de instalar cualquier libreria

Actualizaremos el sistema operativo y los paquetes existentes ejecutando el siguiente comando en la línea de comandos:

```bash
sudo apt update && sudo apt upgrade
```

![](https://github.com/jurado17/DAW/blob/main/Parctica_2_Trimestre/img/c5.png)
Ahora instalamos PHP y los modulos que necesitamos:

```bash
sudo apt install php libapache2-mod-php php-mysql
```

![](https://github.com/jurado17/DAW/blob/main/Parctica_2_Trimestre/img/c6.png)

Verificamos que PHP se haya instalado correctamente con este comando:

```bash
php -v
```

Deberia salir algo como esto

![](/img/c7.png)

Y para que se terminen de aplicar los cambios reiniciamos el servidor Apache

```bash
sudo systemctl restart apache2
```

## Instalación de SQL

Antes de nada actualizamos el servidor

```bash
sudo apt update && sudo apt upgrade
```

E instalamos sql con este comando:

```bash
sudo apt install mysql-server
```

![](/img/c8.png)

Nos pedira una contraseña para el usuario root,le puedes poner la que quieras pero recuerdala,luego iniciaremos el servicio con:

```bash
sudo systemctl start mysql
```

Y comprobamos que se haya instalado correctamente con este comando:

```bash
sudo systemctl status mysql
```

![](/img/c9.png)

## Instalación de PhpMyadmin

Actualizamos el servidor

```bash
sudo apt update
```

E instalamos phpmyadmin con este comando:

```bash
sudo apt install phpmyadmin
```

![](/img/c10.png)

Cuando lo instalemos nos preguntaran si queremos configurarlo le daremos que si y nos dira si queremos intalar la base de datos volveremos a confirmar:

![](/img/c11.png)

Luego nos pedira contraseña la introduciremos y cuando lo hagamos el proceso de instalación habrá acabado y ya tendremos instalado phpmyadmin:

![](/img/c12.png)

Después reiniciamos apache para poder acceder a phpmyadmin

```bash
sudo systemctl restart apache2
```

Después de seguir estos pasos, phpMyAdmin estará instalado y disponible en tu servidor. Puedes acceder a él escribiendo http://localhost/phpmyadmin en tu navegador web y utilizando las credenciales de MySQL para iniciar sesión

## Instalación de un servidor FTP con certificado TLS

Esto es recomendable para evitar que las credenciales y los archivos transmitidos puedan ser interceptados por terceros.

Para instalar primero actualizaremos el sistema operativo haciendo un:

```bash
sudo apt-get update
```

Luego instalaremos el servidor vsftpd:

```bash
sudo apt-get install vsftpd
```

![](/img/c13.png)

### Generar certifiado TLS

Esto es importante para que la conexion sea segura. Para hacerlo usaremos un comando que nos generara un certificado autofirmado que para uso interno es seguro:

```bash
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.pem -out /etc/ssl/private/vsftpd.pem
```

![](/img/c14.png)

Ahora accedemos a la configuracion del servidor:

```bash
sudo nano /etc/vsftpd.conf
```

![](/img/c16.png)

Y añaderemos unas lineas al final del codigo para que los clienten puedan conectarse:

```bash
#Añadiremos estas lineas,si estan ya escritas las modificaremos para que sean iguales a estas
ssl_enable=YES
allow_anon_ssl=NO
force_local_data_ssl=YES
force_local_logins_ssl=YES
ssl_tlsv1=YES
ssl_sslv2=NO
ssl_sslv3=NO
rsa_cert_file=/etc/ssl/private/vsftpd.pem
```

![](/img/c15.png)

Por ultimo reiniciaremos el servicio vsftpd

```bash
sudo systemctl restart vsftpd
```

## Instalación de ssh y ftp

Actulizamos el servidor:

```bash
sudo apt-get update
```

E instalamos el servicio openssh server:

```bash
sudo apt-get install openssh-server
```

![](/img/c18.png)

Accedemos al archivo de configuración para activar el puerto 22 eliminando el #:

```bash
sudo nano /etc/ssh/sshd_config
```

![](/img/c20.png)

![](/img/c19.png)

Por ultimo reiniciaremos el servicio para que aplique las configuraciones y active el puerto

```bash
sudo systemctl restart ssh
```

![](/img/c17.png)

Si tenemos problemas tendremos que ver si el firewall permite las conexiones ssh por ese puerto. En caso de que lo haga, usamos este comando:

```bash
sudo ufw allow ssh
```

Podemos comprobar que todo este correcto usando este comando:

```bash
sudo systemctl status ssh
```

![](/img/c21.png)

## Scripts para automatizar el proceso

### Creacion de usurario y directorio correspondiente para el alojamiento web

Este script te pedira el nombre,el subdominio y la ip para configuraciones de despues y creara el usuario con su directorio,recordar que cuando creamos el usuario si lo hemos instalado y configurado correctatmente se crearan los usuarios ftp y ssh automaticamente.

```bash
	#!/bin/bash

	# Pedir el nombre del usuario,del subdominio y la IP del cliente
	read -p "Ingrese el nombre de usuario: " usuario
	read -p "Ingrese el nombre del subdominio: " subdominio
	read -p "Ingrese la IP del cliente: " ip

	# Crear el usuario del sistema y su directorio correspondiente para alojamiento web y
	# se crea un usuario para acceso a ftp, ssh y smtp
	useradd -m -d /var/www/$subdominio $usuario
```

![](/img/c22.png)

### Creación del host virtual

Esta parte del script creara el archivo del sitio que hemos creado y habilitara el sitio para que se pueda acceder

```bash
	#Creación del archivo de configuración del usuario y luego creación del virtual host
	sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/$nombre.conf
				echo "<VirtualHost *:80>
				    ServerAdmin admin@example.com
				    ServerName $nombre.com
				    ServerAlias www.$nombre.com
				    DocumentRoot /var/www/$nombre
				    ErrorLog ${APACHE_LOG_DIR}/error.log
				    CustomLog ${APACHE_LOG_DIR}/access.log combined
				</VirtualHost>" >> /etc/apache2/sites-available/$nombre.conf
	#Habilitar los virtual hosts
	sudo a2ensite $nombre.conf
```

![](/img/c23.png)

### DNS

Aqui tenemos la parte del DNS que nos generara la zona directa e inversa del subdominio

```bash
	#BIND
	# Añade la zona al archivo named.conf.local
	echo "zone \"$subdominio\" {
	    type master;
	    file \"/etc/bind/db.$subdominio\";
	};" >> cat /etc/bind/named.conf.local

	# Crea el archivo de zona
	sudo cp /etc/bind/db.empty /etc/bind/db.$subdominio
	sudo chown bind:bind /etc/bind/db.$subdominio

	# Añade los registros de resolución directa e inversa al archivo de zona
	echo "\$TTL 86400
	@       IN      SOA     $subdominio.      admin.$subdominio. (
		                2023021801      ; serial
		                3600            ; refresh
		                1800            ; retry
		                604800          ; expire
		                86400 )         ; minimum

		IN      NS      $subdominio.

	$subdominio.       IN      A       $ip" >> cat /etc/bind/db.$subdominio

	echo "$ip       IN      PTR     $subdominio." >> cat /etc/bind/db.10

	#Reinicia el servicio BIND
	sudo systemctl restart bind9
```

![](/img/c24.png)

### Creación de la Base de Datos

Esta parte del script creara un usuario, su base de datos y le dara los privilegios

```bash
	#Crear una base de datos y un usuario con todos los permisos
	mysql -e "CREATE DATABASE $usuario; CREATE USER '$usuario'@'localhost' IDENTIFIED BY 'password';
	GRANT ALL PRIVILEGES ON $usuario.* TO '$usuario'@'localhost'; FLUSH PRIVILEGES;"
```

![](/img/c25.png)

### Habilitar Python

Primero deberemos instalar el modulo wsgi con este comando:

```bash
	sudo apt-get install libapache2-mod-wsgi-py3
```

![](/img/c29.png)

Esta parte del script que habilita y deshabilita el modulo para ejecutar aplicaciones aplicaciones python

```bash
	#Habilitar la ejecución de aplicaciones Python con el servidor web
	a2enmod wsgi

	#Deshabilitar la ejecución de aplicaciones Python con el servidor web
	sudo a2dismod wsgi
```

![](/img/c26.png)


Por ultimo un menu para poder escoger la opción que quiera el usuario

```bash
echo "Buenos dias ¿que desea hacer hoy?"
		echo "1. Crear usuario en Apache"
		echo "2. Crea el virtual host"
		echo "3. Configurar el servidor DNS"
		echo "4. Crear base de datos"
		echo "5. Habilitar ejecución de aplicaciones con Python"
		echo "6. Deshabilitar ejecución de aplicaciones con Python"
		echo "7. Salir"
		echo "Dime tu opcion"
		read opc
		case $opc in
			1)crear_usuario;;
			2)crear_virtualhsot;;
			3)configurar_dns;;
			4)crear_bd;;
			5)habilitar_wsgi;;
			6)deshabilitar_wsgi;;
			7)exit;;
			*)echo "La opción escogida es incorrecta";sleep 3;clear;menu;;
		esac
```

![](/img/c28.png)

### Script completo



```bash
#!/bin/bash
function crear_usuario {
# Pedir el nombre del usuario,del subdominio y la IP del cliente
	read -p "Ingrese el nombre de usuario: " usuario
	read -p "Ingrese el nombre del subdominio: " subdominio
	read -p "Ingrese la IP del cliente: " ip
}

function crear_virtualhost {
# Pedir el nombre del usuario,del subdominio y la IP del cliente
	read -p "Ingrese el nombre de usuario: " usuario
	read -p "Ingrese el nombre del subdominio: " subdominio
	read -p "Ingrese la IP del cliente: " ip
#Creación del archivo de configuración del usuario y luego creación del virtual host
	sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/$nombre.conf
				echo "<VirtualHost *:80>
				    ServerAdmin admin@example.com
				    ServerName $nombre.com
				    ServerAlias www.$nombre.com
				    DocumentRoot /var/www/$nombre
				    ErrorLog ${APACHE_LOG_DIR}/error.log
				    CustomLog ${APACHE_LOG_DIR}/access.log combined
				</VirtualHost>" >> /etc/apache2/sites-available/$nombre.conf
#Habilitar los virtual hosts
	sudo a2ensite $nombre.conf
}

function configurar_dns {
# Pedir el nombre del usuario,del subdominio y la IP del cliente
	read -p "Ingrese el nombre de usuario: " usuario
	read -p "Ingrese el nombre del subdominio: " subdominio
	read -p "Ingrese la IP del cliente: " ip
# Añade la zona al archivo named.conf.local
	echo "zone \"$subdominio\" {
	    type master;
	    file \"/etc/bind/db.$subdominio\";
	};" >> cat /etc/bind/named.conf.local

# Crea el archivo de zona
	sudo cp /etc/bind/db.empty /etc/bind/db.$subdominio
	sudo chown bind:bind /etc/bind/db.$subdominio

# Añade los registros de resolución directa e inversa al archivo de zona
	echo "\$TTL 86400
	@       IN      SOA     $subdominio.      admin.$subdominio. (
		                2023021801      ; serial
		                3600            ; refresh
		                1800            ; retry
		                604800          ; expire
		                86400 )         ; minimum

		IN      NS      $subdominio.

	$subdominio.       IN      A       $ip" >> cat /etc/bind/db.$subdominio

	echo "$ip       IN      PTR     $subdominio." >> cat /etc/bind/db.10

#Reinicia el servicio BIND
	sudo systemctl restart bind9
}

function crear_bd {
# Pedir el nombre del usuario,del subdominio y la IP del cliente
	read -p "Ingrese el nombre de usuario: " usuario
	read -p "Ingrese el nombre del subdominio: " subdominio
	read -p "Ingrese la IP del cliente: " ip
#Crear una base de datos y un usuario con todos los permisos
	mysql -e "CREATE DATABASE $usuario; CREATE USER '$usuario'@'localhost' IDENTIFIED BY 'password';
	GRANT ALL PRIVILEGES ON $usuario.* TO '$usuario'@'localhost'; FLUSH PRIVILEGES;"
}

function habilitar_wsgi {
	a2enmod wsgi
}

function deshabilitar_wsgi {
	sudo a2dismod wsgi
}

function menu
	{
		echo "Buenos dias ¿que desea hacer hoy?"
		echo "1. Crear usuario en Apache"
		echo "2. Crea el virtual host"
		echo "3. Configurar el servidor DNS"
		echo "4. Crear base de datos"
		echo "5. Habilitar ejecución de aplicaciones con Python"
		echo "6. Deshabilitar ejecución de aplicaciones con Python"
		echo "7. Salir"
		echo "Dime tu opcion"
		read opc
		case $opc in
			1)crear_usuario;;
			2)crear_virtualhsot;;
			3)configurar_dns;;
			4)crear_bd;;
			5)habilitar_wsgi;;
			6)deshabilitar_wsgi;;
			7)exit;;
			*)echo "La opción escogida es incorrecta";sleep 3;clear;menu;;
		esac
	}
menu
```
