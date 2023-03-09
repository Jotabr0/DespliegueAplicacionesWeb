## Ejemplo 1: Despliegue de la aplicación Guestbook

La aplicación guestbook por defecto utiliza el nombre redis para conectarse a la base de datos, por lo tanto debemos nombrar al contenedor redis con ese nombre para que tengamos una resolución de nombres adecuada.

Los dos contenedores tienen que estar en la misma red y deben tener acceso por nombres (resolución DNS) ya que de principio no sabemos que ip va a coger cada contenedor. Por lo tanto vamos a crear los contenedores en la misma red:

```bash
$ docker network create red_guestbook
```
![network](practica4ejemplo1network.jpg)

```bash
$ docker run -d --name redis --network red_guestbook -v /opt/redis:/data redis redis-server --appendonly yes
```
```bash
$ docker run -d -p 80:5000 --name guestbook --network red_guestbook iesgn/guestbook
```

![contenedores](practica4ejemplo1contenedores1.jpg)

Probamos y vemos que funciona, conectando con la base de datos

![network](practica4ejemplo1guestbook.jpg)


## Ejemplo 2: Despliegue de la aplicación Temperaturas

Vamos a hacer un despliegue completo de una aplicación llamada Temperaturas. Esta aplicación nos permite consultar la temperatura mínima y máxima de todos los municipios de España. 

Vamos a crear una red para conectar los dos contenedores:

```bash
$ docker network create red_temperaturas
```

Ejecutamos los contenedores:

```bash
$ docker run -d --name temperaturas-backend --network red_temperaturas iesgn/temperaturas_backend

$ docker run -d -p 80:3000 --name temperaturas-frontend --network red_temperaturas iesgn/temperaturas_frontend
```

![network](practica4ejemplo2contenedores.jpg)

La aplicación en funcionamiento: 

![network](practica4ejemplo2temperaturas.jpg)







