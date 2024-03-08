# Instalación de docker

Actualizamos los repositorios e instalamos los paquetes necesarios para instalar desde repositorios HTTPS:

        sudo apt-get update
        sudo apt-get install ca-certificates curl gnupg
![](https://github.com/jurado17/DAW/blob/main/Docker/Practica%201/img/c1.png)
![](https://github.com/jurado17/DAW/blob/main/Docker/Practica%201/img/c2.png)
Añadir la clave GPG oficial de Docker:

```bash
sudo install -m 0755 -d /etc/apt/keyrings
        curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
        sudo chmod a+r /etc/apt/keyrings/docker.gpg
```
![](https://github.com/jurado17/DAW/blob/main/Docker/Practica%201/img/c3.png)
Añadimos el repositorio oficial de Docker:

```bash
echo \
        "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
        $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
        sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
![](https://github.com/jurado17/DAW/blob/main/Docker/Practica%201/img/c4.png)
Instalamos Docker Engine:

```bash
sudo apt-get update
        sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
![](https://github.com/jurado17/DAW/blob/main/Docker/Practica%201/img/c5.png)
Para manejar Docker con un usuario sin privilegio, debemos añadir el usuario al grupo `docker`, para ello:

```bash
sudo usermod -aG docker $USER
```
![](https://github.com/jurado17/DAW/blob/main/Docker/Practica%201/img/c6.png)
Debes volver a iniciar una terminal con el usuario, o ejecutar el siguiente comando:

```bash
su - $USER
```

Y finalmente, comprobamos la versión con la que vamos a trabajar:

```bash
docker --version
        Docker version 24.0.7, build afdd53b
```

![](https://github.com/jurado17/DAW/blob/main/Docker/Practica%201/img/c7.png)
Si usamos debian o ubuntu podemos realizar la instalación de la versión de la comunidad:

```bash
apt install docker.io
```
