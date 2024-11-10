---
tags:
#   - HTML5
  # - JavaScript
  # - CSS
#   - YAML
#   - MkDocs
#   - Python
  - Docker
  - Podman
  # - MarkDown
#   - TypeScript
  # - CSV
#   - Bash
#   - Express
#   - ReactJS
#   - NodeJS
#   - NPM
#   - PNPM
#   - ViteJS
#   - SQLite
  # - SQLAlchemy
  # - MySQL
  # - PostgreSQL
  # - MariaDB
    # - MongoDB
---



# Contenedores


Los contenedores son los objetos que "rodean" las imágenes y que incorporan las configuraciones, rutinas de programa, etc. 


## Creación

El comando `create` (o `container create`) crea un contenedor nuevo basado en la imagen especificada:


=== "Docker"


    ```bash title="Crear contenedor - nombre arbitrario"
    docker create nombre_imagen
    docker container create nombre_imagen
    ```

=== "Podman"


    ```bash title="Crear contenedor - nombre arbitrario"
    podman create nombre_imagen
    docker container create nombre_imagen
    ```

Tras la cración se devuelve un número de identificador (ID) y además se asigna un nombre aleatorio al contenedor. 


Agregando la opción `name` se crea un contenedor nuevo basado en la imagen especificada con el nombre indicado:

=== "Docker"

    ```bash title="Crear contenedor - con nombre"
    docker create --name nombre_contenedor  nombre_imagen
    ```

=== "Podman"


    ```bash title="Crear contenedor - con nombre"
    podman create --name nombre_contenedor nombre_imagen
    ```



!!! warning "Permisos de ejecución en Podman" 


    A diferencia de Docker, 
    Podman no ejecuta sus contenedores con permisos de administrador. 
    Como contrapartida, algunas imágenes
    requieren que el usuario actual tenga permisos de administrador para ejecutar cada contenedor. 
    Para quitar esta restricción,
    a los contenedores se los crea con la opción
    `#!bash --security-opt label=disable`.

    De esta forma, la creación del contenedor queda como:

    ```bash title="Crear contenedor - con nombre"
    podman create --security-opt label=disable --name nombre_contenedor nombre_imagen
    ```
    
    Otras opciones son el uso de opciones como `#!bash --privileged` y 
    `#!bash --security-opt seccomp=unconfined`, las cuales amplían los alcances y permisos del contenedor
    en caso que el *engine* de Podman ya posea permisos de administrador (*root*). 
    Estas opciones son menos recomendables en cuanto a la seguridad del sistema se refiere.



## Arranque


La puesta en marcha de los contenedores se puede hacer en base a su número identificador o en base a su nombre:

=== "Docker"

    ```bash title="Arranque - por ID"
    docker start id_contenedor
    ```

    ```bash title="Arranque - por nombre"
    docker start nombre_contenedor
    ```

=== "Podman"

    ```bash title="Arranque - por ID"
    podman start id_contenedor
    ```

    ```bash title="Arranque - por nombre"
    podman start nombre_contenedor
    ```

Agregando las opciones `-a` (adjuntar) e `-i` (interactivo) se puede ver el log en consola y ejecutar comandos:

=== "Docker"

    ```bash title="Arranque - interactivo"
    docker start -a -i nombre_contenedor
    ```
    Las opciones se pueden indicar juntas:

    ```bash title="Arranque - interactivo"
    docker start -ai nombre_contenedor      
    ```


=== "Podman"

    ```bash title="Arranque - interactivo"
    podman start -a -i nombre_contenedor
    ```
    Las opciones se pueden indicar juntas:

    ```bash title="Arranque - interactivo"
    podman start -ai nombre_contenedor      
    ```



## Detención

El comando `stop` detiene al contenedor indicado:

=== "Docker"

    ```bash title="Detención - por nombre"
    docker stop nombre_contenedor
    ```

=== "Podman"

    ```bash title="Detención - por nombre"
    podman stop nombre_contenedor
    ```



!!! info "Señales de parada"

    En Linux la orden `stop` envía la señal del sistema `SIGTERM`. Si el contenedor no se detuvo en 10 segundos (valor predeterminado) Podman le envía la señal `SIGKILL`, lo cual equivale a lanzar la orden `kill`:

    ```bash title="Detención - SIGKILL"
    podman kill nombre_contenedor
    ```

!!! tip "Otras señales"

    Cabe señalar que el comando `kill` permite mandar otras señales diferentes con ayuda de la opción `--signal`:

    ```bash title="Señales específicas"
    podman kill --signal="SIGUP" nombre_contenedor
    ```
    En el ejemplo, la señal `SIGUP` hace que la aplicación relea sus archivos de configuración, siempre y cuando la aplicación lo soporte.


## Listado

El comando `ps` indica los contenedores en funcionamiento:


=== "Docker"

    ```bash title="Listado - contenedores en funcionamiento"
    docker ps
    ```
=== "Podman"

    ```bash title="Listado - contenedores en funcionamiento"
    podman ps
    ```


Con la opción `-a` se indican todos los contenedores existentes:

=== "Docker"

    ```bash title="Listado - todos los contenedores"
    docker ps -a
    ```

=== "Podman"

    ```bash title="Listado - todos los contenedores"
    podman ps -a
    ```




## Borrado

El comando `rm` elimina el contenedor indicado:

=== "Docker"

    ```bash title="Borrado de contenedor"
    docker rm nombre_contenedor
    ```

=== "Podman"

    ```bash title="Borrado de contenedor"
    podman rm nombre_contenedor
    ```



## Referencias


[Red Hat - How to use the `--privileged` flag with container engines](https://www.redhat.com/en/blog/privileged-flag-container-engines)

[Red Hat - How to use Podman inside of a container](https://www.redhat.com/en/blog/podman-inside-container)