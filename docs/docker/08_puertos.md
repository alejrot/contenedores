---
tags:
#   - HTML5
  # - JavaScript
  # - CSS
#   - YAML
  - MkDocs
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
  - MongoDB
---

# Puertos

## Puertos 



Los contenedores se comunican con el resto del software mediante el protocolo TCP/IP,
implementando *sockets* ("zócalos").

<!-- Cada contenedor se comunica con otros contenedores y aplicaciones usando *sockets* , basados en el   -->

Los *sockets* se componen por una dirección IP y por un número de puerto. 
Cada imagen tiene un número de puerto preasignado, 
el cual es heredado del software interno 
y cuyo contenedor hereda tabién de manera predeterminada.  
Por este motivo varios contenedores activos pueden heredar un mismo número de puerto, 
por ejemplo si se manejan imágenes de un mismo *framework* en distintas versiones 
o si se trata de una misma imagen siendo usada para varios programas distintos. 
Para evitar ambigüedades y poder comunicarse con todos ellos los puertos de los contenedores se pueden “mapear” a distintos puertos del sistema anfitrión.



## Port Mapping


Dos o más imágenes activas pueden tener un mismo número de puerto *huesped*.

Dos contenedores activos no pueden tener un mismo número de puerto para comunicarse con el *host*. 



Crea el contenedor ya mapeado al puerto host deseado. (  `-p`: *publish* ). 
**Importante:**
no poner espacios entre números de puerto y signo `:`.

=== "Docker"

    ```bash title="Contenedor - con port mapping"
    docker create -p puerto_anfitrion:puerto_imagen --name nombre_contenedor  nombre_imagen
    ```

=== "Podman" 

    ```bash title="Contenedor - con port mapping"
    podman create -p puerto_anfitrion:puerto_imagen --name nombre_contenedor nombre_imagen
    ```

!!! example "Material for MkDocs"

    Este [framework](https://hub.docker.com/r/squidfunk/mkdocs-material) que renderiza documentos en formato Markdown implementa su *live server* (servidor para pruebas) locales el puerto 8000. 
    Por ello, los contenedores creados para implementar dicho server deben mapear al puerto 8000:

    === "Docker"

        ```bash title="mkdocs-material - live server"
        docker pull mkdocs-material    
        cd ruta_proyecto
        docker create --name live_server -p puerto_host:8000 -v ${PWD}:/docs mkdocs-material
        ```

    === "Podman" 

        ```bash title="mkdocs-material - live server"
        podman pull mkdocs-material   
        cd ruta_proyecto
        podman create --name live_server -p puerto_host:8000 --security-opt label=disable -v ${PWD}:/docs mkdocs-material
        ```

    **Aclaración:** la opción `-v ${PWD}:/docs` es propia del framework.
    Es la ruta relativa a los documentos internos del proyecto actual.



!!! info  "Nº de puerto único"

    Si se indica un solo número de puerto 
    entonces la máquina de contenedores asumirá que éste es puerto de la imagen y entonces elegirá el puerto de *host* arbitrarimente.

    Por ejemplo, MongoDB usa el puerto `27017`. 
    Al indicar dicho numero durante la creación del *container* y al no especificar el puerto de host el *engine* asigna un puerto de *host* arbitrario como el `46665` o el`52043`: 

    === "Docker"

        ```bash
        docker create  -p27017 --name mong_p1    mongo
        ```

    === "Podman" 
      
        ```bash
        podman create  -p27017 --name mong_p1    mongo
        ```

    En general es mejor especificar ambos puertos al crear contenedores con *port-mapping*.




