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
  - MongoDB
---

# Puertos

### Puertos y Port Mapping

Los contenedores se comunican con el resto del software mediante el protocolo TCP/IP.

Cada contenedor tiene un número de puerto asignado, eso le permite la comunicación con otros contenedores y aplicaciones usando sockets. Dos contenedores activos pueden tener un mismo número de puerto. Para evitar ambigüedades y poder comunicarse con ambos los puertos de los contenedores se pueden “mapear” a distintos  puertos del anfitrión

Crea el contenedor ya mapeado al puerto host deseado. (  `-p`: *publish* ). 
**Importante:**
no poner espacios entre números de puerto y signo `:`.

```bash
docker create  -p puerto_anfitrion:puerto_imagen --name nombre_contenedor  nombre_imagen
```

**TIP**: Si se indica un solo número de puerto 
entonces Docker asumirá que éste es puerto de la imagen y entonces elegirá el puerto de host arbitrarimente:

Ej: MongoDB usa el puerto `27017` y al no especificar el puerto de host Docker asigna un puerto externo como el `52043` 
```bash
docker create  -p27017 --name mong_p1    mongo
```


