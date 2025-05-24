
# Sintaxis básica


## Nombre

Con el parámetro opcional `name` se puede elegir el nombre del proyecto:


```yaml
name: nombre_proyecto  # (opcional)
```

Este valor puede ser consultado con una variable de entorno llamada
`COMPOSE_PROJECT_NAME`.

## Servicios

La sección `services` es la indicada
para definir los contenedores del proyecto.

A cada contenedor se le asigna un nombre de servicio,
a eleccion del desarrollador:

```yaml
name: nombre_proyecto  # (opcional)

services:

  # primer contenedor
  nombre_servicio_1:  
    # parámetros del contenedor
    # ...

  # segundo contenedor
  nombre_servicio_1:  
    # parámetros del contenedor
    # ...

  # ...
```

Dentro de cada bloque de servicio creado
se definen los valores de los parámetros internos para cada contenedor.


### Nombre de contenedor

El parámetro opcional `container_name` permite elegir un nombre
para cada contenedor.

```yaml
services:
  nombre_servicio:
    container_name: nombre_contenedor
```
Si este parámetro no es especificado
entonces el gestor elige como nombre
una variante del nombre del proyecto
combinada con el nombre de servicio.



### Imagen predefinida

El campo `image` es el encargado de elegir una imagen preconstruida
para ser desplegada como contenedor:

```yaml
services:
  nombre_servicio:
    image: nombre_imagen
```

De manera predefinida la imagen elegida será la etiquetada como `latest` (última).
Para elegir otra variante de la imagen requerida ésta se indica precedida por dos puntos (`:`):


```yaml
services:
  nombre_servicio:
    image: nombre_imagen:tag_version
```


Si la imagen elegida no está disponible localmente entonces ésta se descarga de alguno de los servidores habilitados para tal fin.


### Construcción

El archivo Compose permite automatizar la construcción de imágenes modificadas.

El campo `build` permite indicar la ruta al directorio
donde exista un archivo Dockerfile.


```yaml
services:
  nombre_servicio:
    build: .  # Dockerfile aledaño al archivo Compose
```

Este archivo sólo se utilizará la primera vez que se realice el despliegue.


!!! tip "`image` vs `build`"

    Los parámetros `image` y `build` pueden usarse juntos
    para construir una imagen modificada según el Dockerfile especificado por `build`
    y asignarle el nombre y etiqueta indicados por `image`. 



### Comandos

El campo `command` permite sobreescribir el comando (`CMD`)
predefinido dentro del Dockerfile de la imagen usada,
reemplazándolo por la expresiópn elegida.



## Ejemplo de uso: BusyBox

[BusyBox](https://www.docker.com/blog/use-cases-and-tips-for-using-the-busybox-docker-official-image/) es una imagen compacta que dispone de múltiples comandos básicos de la *shell*.

En este ejemplo se crea un contenedor único que escribe en consola el nombre del proyecto mediante el parámetro `command`:


```yaml
name: comando_busybox

services:
  contenedor_basico:
    container_name: demo_busybox
    image: busybox:1.37.0-glibc
    command: echo "Me llamo '${COMPOSE_PROJECT_NAME}'"
```

Más versiones de BusyBox: [Docker Hub](https://hub.docker.com/_/busybox)

## Referencias

[Docker Docs - Version and name top-level elements](https://docs.docker.com/reference/compose-file/version-and-name/)

[Docker.com - How to Use the BusyBox Docker Official Image](https://www.docker.com/blog/use-cases-and-tips-for-using-the-busybox-docker-official-image/)


[Docker Hub - BusyBox](https://hub.docker.com/_/busybox)
