# Extras


## Compartir imagen en Docker Hub

Nos identificamos para que el servidor nos permita subir la imagen a nuestro nombre

```bash
docker login
```

Se indica el nombre de nuestra imagen personalizada, 
su número de versión y se etiqueta con el grupo de imágenes al que pertenece 

```bash
docker tag <grupo_imagenes> <mi_imagen>:<version>
```

**Ejemplo:** 
si nuestra imagen es una actualización de NodeJS la etiquetamos como NodeJS de modo que aparezca publicada con las demás versiones.

Ordenamos el envío de nuestra imagen al servidor para publicarla:
```bash
docker push <mi_imagen>:<version>
```



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
# ls
```




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



