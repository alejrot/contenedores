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

Los argumentos permiten asignar valores a algunos parámetros internos del Dockerfile.
Permiten pasar desde la *shell* tanto variables comunes como a variables de entorno.

## Variables

Se definen dentro del Dockerfile 
con la cláusula `ARG` 
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


## Variables de entorno

Las variables de entorno usan la cláusula `ENV`.
Estas variables pueden apoyarse en valores de argumentos
tanto para recibir los valores por defecto 
como para ser modificadas durante la construcción:

```Dockerfile title=""
ARG NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}
```