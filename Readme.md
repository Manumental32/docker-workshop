Intro teorica

- Dockerfile es la receta para construir (build) un container
- Hay un solo CMD por Dockerfile, el CMD de mi Dockerfile pisa al DockerFile de python en este caso, lo mismo pasa con el ENTRYPOINT que es lo primero que se corre
- VOLUMEN para compartir directorios, por ejemplo para copiar una carpeta adentro. del container, en tiempo de ejecución, a diferencia del COPY
- El caso 2 es mas prolijo porque tiene un WORKDIR que le dice donde pararse
- El RUN del Dockerfile se ejecuta en el momento del bildeo

## section 1:

Construir una imagen de docker con python3 que corre con CMD un echo

topics: build, run, -t, images, FROM, CMD, layers

1) Construyo y tageo la imagen con el nombre docker_workshop_1
> docker build . -t docker_workshop_1

2) Corro la imagen (la convierto en un container)
> docker run docker_workshop_1

3) Veo el listado de imagenes para ver que efectivamente se creo y el status es running
> docker images


## section 2:
topics: ENV, -e, CMD

Construir una imagen con un ENV default y despues cambiarlo en runtime

1) Contruyo la imagen con un ENV ya seteado en el dockerfile (default)
> docker build . -t docker_workshop_2

2) Le cambio el ENV con -e en runtime
> docker run -e 'ATRIBUTO=KAPO' docker_workshop_2

3) Veo el listado de imagenes para ver que efectivamente se creo
> docker images

## section 3:

Construir una imagen que hace un COPY de una carpeta local al container y despues corre el comando (CMD) "phython <path_del_ejecutable_de_python.py>

topics: COPY, ENTRYPOINT, CMD

> docker build . -t docker_workshop_3

> docker run docker_workshop_3

> docker images

## section 4:
topics: WORKDIR, RUN, -f file, taging, rmi

Construir una imagen con un Dockerfile_bis (con -f) que tambien hace un COPY de una carpeta local al container pero con WORKDIR le defino el espacio de trabajo.
Tambien vimos:  
- El rmi para eliminar containers por tag
- El : para tagear versiones

> docker build . -f Dockerfile_bis -t docker_workshop_3:latest

> docker run docker_workshop_4

> docker build . -t docker_workshop_4:bis

> docker run docker_workshop_4:bis

> docker images

> docker tag docker_workshop_4:bis docker_workshop_4:good

> docker rmi docker_workshop_4:bis

> docker run docker_workshop_4:good


## section 5:
topics: first api, --name my_flask_app, ps, exec, kill, stop, port 

Construir una imagen que corre python pip install con los requirments del .txt. 



> docker build . -t docker_workshop_5

Pongo a correr con un nombre especifico con --name
> docker run --name my_flask_app docker_workshop_5

Listar los containers corriendo
> docker ps 

Correr bash dentro del container
> docker exec -ti my_flask_app bash

Matar el proceso dentro del container
> docker kill my_flask_app 

Freno el container
> docker stop my_flask_app 

Lo pongo a correr nuevamente
> docker run --name my_flask_app docker_workshop_5

Freno el container
> docker stop my_flask_app 

Elimino el container
> docker rm my_flask_app

> curl 127.0.0.1:5000

Abro bash dentro del container
> docker exec -ti my_flask_app bash

Hago un curl dentor del bash
> curl 127.0.0.1:5000

Expongo el puerto -p :5000 para poder verlo desde afuera
> docker run --name my_flask_app_exp_port -p 0.0.0.0:5000:5000  docker_workshop_5

Lo pruebo
> curl 127.0.0.1:5000


## section 6:
topics: volumes (-v), hot reload, run as daemon, logs (-d)

Agrego en el dockerfile:
ENV FLASK_ENV=development
para hacer un hot reload de los cambios

> docker build . -t docker_workshop_6

> docker run --name my_flask_app_with_reload -p 0.0.0.0:5000:5000 -v $PWD/app:/app docker_workshop_6

> curl 127.0.0.1:5000

> #make some change on the code, and view what is happening

> curl 127.0.0.1:5000

> docker rm -f my_flask_app_with_reload

Guardar los logs con -d
> docker run -d --name my_flask_app_with_reload -p 0.0.0.0:5000:5000 -v $PWD/app:/app docker_workshop_6

Leer los logs
> docker logs -f my_flask_app_with_reload


## section 7
topics: push and pull

Subir y bajar imagenes de docker hub

> #be sure you have a free account on https://hub.docker.com/

> docker login

> docker tag docker_workshop_6 pabloncio/docker_workshop:1.0.0 

> docker push

> docker rm -f  $(docker ps -aq)

> rmi -f $(docker images -aq)

> docker pull pabloncio/docker_workshop:1.0.0
# docker-workshop
