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


# Logs

Da los reportes del container usado por terminal 
y devuelve el control de la terminal al usuario:
```bash
docker logs nombre_contenedor
```

Da los reportes del container elegido por terminal
 y espera a nuevos sucesos del mismo:
```bash
docker logs --follow nombre_contenedor
```

**HINT**: 
se sale del modo live con ++ctrl+c++.

