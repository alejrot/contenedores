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


# Comando `run`

```bash
docker run nombre_imagen
```
Combina varios comandos:

- Busca la imagen, y si ésta no está disponible localmente la descarga;
- Crea un contenedor que incluye la imagen elegida;
- Inicia el contenedor creado.

Esta instrucción no devuelve el control al usuario a menos que termine 
ó se presione ‘ctrl’+’c’ (cancelación). 
Si se repite el comando varias veces se crean varios contenedores parecidos, uno por comando.

Lo mismo que antes, pero la opción `-d` (*dettached*) devuelve el control al usuario de inmediato.
```bash
docker run  -d nombre_imagen
```

Lo mismo pero añadiendo el *port mapping*:

```bash
docker run --name nombre_contenedor -p puerto_anfitrion:puerto_imagen -d nombre_imagen
```