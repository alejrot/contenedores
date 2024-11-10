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


# Logs



El comando `logs` da los reportes del *container* usado hasta el presente y devuelve el control de la terminal al usuario:

=== "Docker"

    ```bash title="mensajes de log"
    docker logs nombre_contenedor
    ```

=== "Podman" 

    ```bash title="mensajes de log"
    podman logs nombre_contenedor
    ```

Agregando la opción `--follow` la terminal queda en espera a nuevos eventos:

=== "Docker"

    ```bash title="mensajes de log - modo live"
    docker logs --follow nombre_contenedor
    ```

=== "Podman" 

    ```bash title="mensajes de log - modo live"
    podman logs --follow nombre_contenedor
    ```



!!! hint "Atajo de salida"
    Se sale del *modo live* con ++ctrl++ + ++c++


