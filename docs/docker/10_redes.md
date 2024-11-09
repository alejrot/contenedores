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
#   - MongoDB
---


# Redes en Docker

Es posible interconectar los contenedores mediante redes puente (*bridge*) creadas en Docker.

Enumera las redes creadas por los contenedores

```bash
docker network ls
```

Crea una nueva red del tipo bridge (puente) con el nombre especificado:
```bash
docker network create nombre_red
```
Elimina la red indicada:
```bash
docker network rm nombre_red
```

Crea un contenedor que incluya conexión a la red *bridge* indicada. 
La red puente se indica como una opción más:
```bash
docker create [...]  --network nombre_red  [..]
```

Para que los contenedores funcionen es necesario que las redes que utilizan hayan sido creadas previamente.

