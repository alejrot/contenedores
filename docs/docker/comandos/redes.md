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
#   - MongoDB
---


# Redes Bridge
<!-- 
Es posible interconectar los contenedores mediante redes puente (*bridge*) creadas en Docker. -->

Los contenedores se interconectan mediante redes puente (*bridge)*.


## Listar redes

Enumera las redes creadas por los contenedores

=== "Docker"

    ```bash title="Redes - listar"
    docker network ls
    ```
=== "Podman" 

    ```bash title="Redes - listar"
    podman network ls
    ```

## Crear red

Crea una nueva red del tipo bridge (puente) con el nombre especificado:

=== "Docker"

    ```bash title="Redes - crear"
    docker network create nombre_red
    ```

=== "Podman" 

    ```bash title="Redes - crear"
    podman network create nombre_red
    ```

## Eliminar red

Elimina la red indicada:

=== "Docker"

    ```bash title="Redes - eliminar"
    docker network rm nombre_red
    ```

=== "Podman" 

    ```bash title="Redes - eliminar"
    podman network rm nombre_red
    ```

## Contenedores conectados

Crea un contenedor que incluya conexión a la red *bridge* indicada. 
La red puente se indica como una opción más:


=== "Docker"

    ```bash title="Contenedores - conectados a red"
    docker create   --network nombre_red  --name nombre_contenedor  nombre_imagen
    ```

=== "Podman" 

    ```bash title="Contenedores - conectados a red"
    podman create   --network nombre_red   --name nombre_contenedor  nombre_imagen
    ```

!!! warning "Redes preexistentes"

    Para que la comunicación entre contenedores funcione es necesario que las redes que estos utilizan hayan sido creadas previamente.



## Referencias


[Repositorio ofical de Podman - Networking básico](https://github.com/containers/podman/blob/main/docs/tutorials/basic_networking.md)


