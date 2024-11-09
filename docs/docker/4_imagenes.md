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
---




# Imágenes

Indica qué imágenes están descargadas en la PC y sus características básicas: versión, identificador, espacio ocupado, etc.
```bash
docker images
```

Descarga la última versión disponible de la imagen indicada desde docker hub y la etiqueta como `latest`:

```bash
docker pull nombre_imagen
```

Descarga la versión indicada de la imagen  y la etiqueta numerada.

```bash
docker pull nombre_imagen:version
```

Si no se indica la versión se descarga la más reciente (`latest`):

En Docker Hub se puede consultar qué imágenes hay disponibles e indica el comando completo para descargarlas. 

**Importante:** 
hay que chequear también los comandos para configurar los futuros contenedores basados en estas imágenes.

Elimina del disco la imagen especificada:
```bash
docker image rm <nombre-imagen>:<numero-version>
```
