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



# Archivo Dockerfile

El archivo Dockerfile nos permite crear nuevas imágenes en base a una preexistente donde se incluyan nuevos comandos y aplicaciones. A este archivo no se le puede cambiar el nombre.

### 1.Sintaxis

La sintaxis es la siguiente:

```Dockerfile
FROM imagen_base:version		(qué imagen de Docker será afectada)

RUN mkdir - p <ruta_destino>		(se crea el directorio de destino)(tipicamente `/home/app` si es contenedor con Linux)

RUN <comandos_instalacion_aplicacion>  (opcional)

COPY . <ruta_destino>		(completar con la ruta que corresponda)

EXPOSE <numero_puerto>			(indica el puerto del contenedor)

CMD [ <comando> , <ruta_destino/ejecutable> ]
```

El comando `RUN` permite instalar aplicaciones dentro de la imagen de ser necesario. 

Es posible indicar un directorio de funcionamiento con el comando `WORKDIR`, de este modo la sintaxis quedaría así:

```Dockerfile
[...]

WORKDIR ruta_destino

[...]

CMD [comando , ejecutable]
```

### 2. Nuevas imágenes modificadas

Construir imagen desde Dockerfile: 
```bash
docker build -t nombre_aplicacion:numero_version  ruta_a_archivos
```
Construye una imagen a partir de un archivo Dockerfile disponible en la ruta indicada. 
Si la terminal ya está ubicada en el directorio raíz de la aplicación hay que poner un punto. 
Ejemplo: `docker build -t miapp:1 .`

Lo mismo pero en directorio actual:
```bash
docker build -t nombre_aplicacion:numero_version  .
```
El archivo Dockerfile debe estar allí.

