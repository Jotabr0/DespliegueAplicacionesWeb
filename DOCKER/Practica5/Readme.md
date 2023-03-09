## COMPOSE 

 ## Instalación de docker-compose

```bash
apt install docker-compose
```

Crearemos también un archivo docker-compose.yml donde añadires el código necesario para desplegar el conjunto de contenedores y servicios que necesitemos.

## Ejemplo 1 - Guestbook

Añadiremos a docker-compose.yml:

```bash
version: '3.1'
services:
  app:
    container_name: guestbook
    image: iesgn/guestbook
    restart: always
    ports:
      - 80:5000
  db:
    container_name: redis
    image: redis
    restart: always
```

![compose](ejemplo1visual.jpg)

Creamos el escenario:

```bash
$ docker-compose up -d
Creating network "guestbook_default" with the default driver
Creating guestbook ... done
Creating redis     ... done
```

Comprobamos los contenedores:

```bash
$ docker-compose ps
```
![compose](ejemplo1contenedores.jpg)

Por último comprobamos el despligue de la aplicación en nuestro navegador:

![compose](ejemplo1navegador.jpg)

Luego para parar los contenedores:

```bash
$ docker-compose stop 
```

Y para eliminar el escenario:

```bash
$ docker-compose down
```

![compose](ejemplo1stop.jpg)




