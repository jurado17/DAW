# El "Hola Mundo" de docker

Vamos a comprobar que todo funciona creando nuestro primer contenedor desde la imagen `hello-world`:

```bash
$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
0e03bdcc26d7: Pull complete 
Digest:     sha256:31b9c7d48790f0d8c50ab433d9c3b7e17666d6993084c002c2ff1ca09b96391d
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working     correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs    the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which  sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```


Si listamos los contenedores que se están ejecutando (`docker ps`):

```bash
$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```
Comprobamos que este contenedor no se está ejecutando. **Un contenedor ejecuta un proceso y cuando termina la ejecución, el contenedor se para.**

Para ver los contenedores que no se están ejecutando:

```bash
$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
372ca4634d53        hello-world         "/hello"            8 minutes ago       Exited (0) 8 minutes ago                       elastic_johnson
```
