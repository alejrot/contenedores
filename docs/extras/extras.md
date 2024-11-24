# Extras


## Nombrar imagenes 

las imágenes ya creadas se renombran con el comando `tag`:

```
docker tag nombre_actual_imagen   nuevo_nombre_imagen:version
docker tag id_imagen              nuevo_nombre_imagen:version
```

<!-- 
Se indica el nombre de nuestra imagen personalizada, 
su número de versión y se etiqueta con el grupo de imágenes al que pertenece 

```bash
docker tag <grupo_imagenes> <mi_imagen>:<version>
``` 
**Ejemplo:** 
si nuestra imagen es una actualización de NodeJS la etiquetamos como NodeJS de modo que aparezca publicada con las demás versiones. 
-->


## Compartir imagen en Docker Hub

Nos identificamos para que el servidor nos permita subir la imagen a nuestro nombre

```bash
docker login
```



Ordenamos el envío de nuestra imagen al servidor para publicarla:
```bash
docker push <mi_imagen>:<version>
```

INFO: Docker Hub da alojamiento gratuito a las imágenes
que sean compartidas públicamente por los usuarios.


## Ejecutar aplicaciones internas

[DOCKER De NOVATO a PRO! (CURSO COMPLETO EN ESPAÑOL)](https://youtu.be/CV_Uf3Dq-EU?list=PLEI4UgZ2UclHp7pugJExZhRUGBuNhMGsN&t=1166)

Ejecuta un comando o aplicación interno del contenedor:

```bash
docker exec <nombre_contenedor> <nombre comando>
```

Ejecuta un comando o aplicación del contenedor de forma interactiva:

```bash
docker exec -it <nombre_contenedor> <nombre comando>
```

Ejemplo: abrir una shell del contenedor y explorar archivos internos

```bash
docker exec -it <contenedor> sh  
ls
```

## CMD, RUN y ENTRYPOINT en Dockerfile


[DIFERENCIA entre CMD, RUN, y ENTRYPOINT en DOCKER - V2M](https://www.youtube.com/watch?v=6ZnecM3ipu4)


- `RUN`: permite ejecutar un comando en específico durante la creacion de la imagen.
Ejemplo:
    ```dockerfile
    RUN pip install fastapi
    ```
- `CMD`: ejecuta una rutina interna especifica al arrancar el contenedor.
Ejemplo: un servidor hecho en Python
    ```dockerfile
    CMD ["python", "/usr/src/myapp/server.py"]
    ```

- `ENTRYPOINT`: llama al ejecutable interno al arrancar el contenedor, 
pero dependiendo de uno o varios argumentos externos.
Ejemplo: el intérprete de Python encapsulado en un contenedor.

    ```dockerfile
    ENTRYPOINT ["python"]
    ```

El contenedor ejecuta la rutina que se le indique como argumento:

    ```bash
    docker run contenedor-python  ruta_rutina.py
    ```


## `build` con y sin nombre

```bash
docker build  .
podman build -t nombre_imagen  .
```


## `run` con ruta a volumnen

Es la opción  `-v`
```bash
docker run -d   -v ruta_anfitrion:ruta_interna  ... nombre_imagen
```
pueden asignarse varios volumenes con la opcion `-v`:

```bash
docker run -d   -v ruta_host_1:ruta_interna_1   -v ruta_host_2:ruta_interna_2 ... nombre_imagen
```

TIP: pasar las rutinas adentro de los contenedores mediante volumenes.
Esto evita tener que reconstruir los containers cada vez que se hagan cambios en las rutinas.


(REVISAR)



## Multiples contenedores


Cuando hay que conectar varios contenedores interdependientes conviene usar las *networks*.

```bash
docker network create nombre_red
```

Al crear un contenedor nuevo se incluyen las opciones:

- `--network` :   especifica el nombre de red (preexistenta) a conectar;
- `--network-alias`: funciona como un apuntador de la red al contenedor destino de la red

Las redes de Docker no necesitan configuración de puertos para funcionar.

INFO: en los archivos `docker-compose.yml` las redes y sus alias se crean automáticamente


## MAS: [Optimizing Docker Image Sizes: Advanced Techniques and Tools](https://devdojo.com/bobbyiliev/optimizing-docker-image-sizes-advanced-techniques-and-tools)

- construccion de imagenes en dos fases
- archivo dockerignore
- optimizaciones por lenguaje
- y mucho mas


## Referencias


[Alternativas a Docker](https://www.cloudzero.com/blog/docker-alternatives)

[CRI - Alternativa a Kubernetes](https://cri-o.io)

[RUNC , OCI y CRI](https://www.tutorialworks.com/difference-docker-containerd-runc-crio-oci/)

[Open Container Initiative](https://opencontainers.org/about/overview/)

FlatPak (no requiere daemon)

[FlatPack vs Snap](https://blog.desdelinux.net/flatpak-vs-snap/)

[Podman (Docker Sin Daemon)](https://podman.io/whatis.html)


[Las ventajas de Podman sobre Docker](https://pandorafms.com/blog/what-is-podman/)

[Podman Desktop](https://podman-desktop.io/downloads/Linux)

[Usando el Docker Compose en Podman](https://www.redhat.com/sysadmin/podman-docker-compose)

[Red Hat - Guia de contenedores](https://access.redhat.com/documentation/es-es/red_hat_enterprise_linux/8/html/building_running_and_managing_containers/index)



