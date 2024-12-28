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
---


# Ejecutar en contenedor


Con el comando `exec` se puede ingresar un contenedor específico desde la terminal para poder realizar diversas operaciones: instalar nuevas dependencias, leer logs, explorar el interior del contenedor, etc.


## Comando único


Para ejecutar un comando o aplicación interno del contenedor se especifica el nombre o el ID del contenedor elegido 
y se indica el comando al final: 


=== "Docker"

    ```bash title="Comando interno"
    docker exec nombre_contenedor nombre_comando ...
    docker exec     id_contenedor nombre_comando ...
    ```
=== "Podman"

    ```bash title="Comando interno"
    podman exec nombre_contenedor nombre_comando ...
    podman exec     id_contenedor nombre_comando ...
    ```

## Modo interactivo

Si se busca ingresar al contenedor para gestionarlo desde la terminal
entoces se agrega la opción `-it` y al final se ordena el comando `sh` (*shell*):

=== "Docker"

    ```bash title="Shell interna"
    docker exec -it nombre_contenedor sh
    docker exec -it     id_contenedor sh
    ```

=== "Podman"

    ```bash title="Shell interna"
    podman exec -it nombre_contenedor sh
    podman exec -it     id_contenedor sh
    ```

de esta forma queda abierta la sesión dentro de la terminal actual hasta ingresar el comando `exit`.