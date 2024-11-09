---
tags:
#   - HTML5
  # - JavaScript
  # - CSS
#   - YAML
#   - MkDocs
#   - Python
  - Docker
#   - Podman
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

### Contenedores

Crea un contenedor nuevo basado en la imagen especificada. 

```bash
docker create nombre_imagen
docker container create nombre_imagen
```

se devuelve un número de identificador (ID) y además se asigna un nombre aleatorio al contenedor. 

Crea un contenedor nuevo basado en la imagen especificada con el nombre indicado:
```bash
docker create --name nombre_contenedor  nombre_imagen
```
Pone en funcionamiento el contenedor identificado por ID:
```bash
docker start ID
```
Pone en funcionamiento el contenedor indicado por nombre:

```bash
docker start nombre_contenedor
```

Detiene al contenedor indicado

```bash
docker stop nombre_contenedor
```

Indica los contenedores en funcionamiento.
```bash
docker ps
```
Indica todos los contenedores existentes
```bash
docker ps -a
```
Elimina el contenedor indicado
```bash
docker rm nombre_contenedor
```

