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

# Argumentos

Los argumentos permiten asignar valores variables a algunos parámetros.

## Creación

Se definen dentro del Dockerfile 
con la orden `ARG` 
y traen un valor predefinido:


```Dockerfile title="Argumentos - definición"
ARG VERSION=latest
FROM imagen_base:${VERSION}
#...
```


## Modificación

Los valores de los argumentos
pueden ser modificados durante la construcción de la imagen con la opción `--build-arg`:

=== "Docker"

    ```bash title="Argumentos - nuevo valor"
    docker build --build-arg VERSION=18.04 .
    ```

=== "Podman"

    ```bash title="Argumentos - nuevo valor"
    podman build --build-arg VERSION=18.04 .
    ```


## Visibilidad

Cada variable debe ser redefinida en cada etapa (*stage*) implementada dentro del Dockerfile.
