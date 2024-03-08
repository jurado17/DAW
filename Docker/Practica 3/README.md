# Práctica 3. Docker
## Descarga imagenes
Para esta práctica vamos a descargar las siguiente imagenes:

Ubuntu

````bash
sudo docker pull ubuntu
````
![](https://github.com/jurado17/DAW/blob/main/Docker/Practica%203/img/c1.png)
Hello-world

````bash
sudo docker pull hello-world
````
![](https://github.com/jurado17/DAW/blob/main/Docker/Practica%203/img/c2.png)
Nginx

```bash
sudo docker pull nginx
```
![](https://github.com/jurado17/DAW/blob/main/Docker/Practica%203/img/c3.png)
## Mostrar todas las imagenes
Para mostrar todas las imágenes que tenemos instaladas vamos a usar el siguiente comando:

```bash
sudo docker images
```
![](https://github.com/jurado17/DAW/blob/main/Docker/Practica%203/img/c4.png)

## Ejecutar contenedores
Vamos a crear y ejecutar 3 contenedores con la imagen getting-started debido a que la imagen hello-world solo devuelve un mensaje de bienvenida y se para solo, necesitamos que se muestren los contenedores corriendo.

Para crear los contenedores vamos a usar el comando docker run, este crea e inicia directamente el contenedor.

```bash
sudo docker run --name myhello1 -d getting-started

sudo docker run --name myhello1 -d getting-started

sudo docker run --name myhello1 -d getting-started
```
![](https://github.com/jurado17/DAW/blob/main/Docker/Practica%203/img/c5.png)

## Muestra los contendores que se están ejecutando
Para ver los contendores que se están ejecutando vamos a usar el comando docker ps

Los contendores que se están ejecutando
![](https://github.com/jurado17/DAW/blob/main/Docker/Practica%203/img/c7.png)
## Parar y borrar contendores
Vamos a parar los contendores myhello1 y myhello2, eliminar myhello1 y por último mostrar los contendores que se están ejecutando.

Primero vamos a parar los contenedores.

```bash
sudo docker stop myhello1

sudo docker stop myhello2
```

![](https://github.com/jurado17/DAW/blob/main/Docker/Practica%203/img/c8.png)
A continuación eliminamos el contenedor.

```bash
sudo docker rm myhello1
```

![](https://github.com/jurado17/DAW/blob/main/Docker/Practica%203/img/c9.png)
Por último, mostramos el historial de contendores con ps -a para poder ver que ya no hay ningún contenedor myhello1, además que el único que sigue ejecutandose es myhello3.


## Eliminar todos los contenedores
Para elimar los contendores primero necesitamos pararlos, si no, nos daré una excepción.

```bash
sudo docker rm myhello2

sudo docker stop myhello3

sudo docker rm myhello3
```
![](https://github.com/jurado17/DAW/blob/main/Docker/Practica%203/img/c10.png)
