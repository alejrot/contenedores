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





# Docker Compose


Este comando habilita usar una plantilla para facilitar  la creación de los contenedores y su interconexión. El archivo de configuración se llama `docker-compose.yml`   (archivo YAML).
Dentro del archivo debe respetarse estrictamente la indentación sino no funciona.
También hay que respetar las comillas para los números de puerto y los guiones donde se indican. 
Docker Compose aprovecha el archivo Dockerfile por defecto para completar la configuración del proyecto.
Los comentarios son precedidos del carácter `#`

## Sintaxis

Sintaxis (ejemplo):
```yaml
version:  “numero_version”

services:

	contenedor_1:
		build:  ruta_Dockerfile 
		ports:
			# lista de mapeos
			- “puerto_host_1:puerto_imagen_1”
			- “puerto_host_2:puerto_imagen_2” 
							#respetar las comillas dobles para los puertos
 			[...]		#(pueden mapearse tantos puertos como se necesite )
		links:
 			- contenedor_2 		#contenedor destino

	contenedor_2:
		image: imagen_2		# (Imagen preexistente)
		ports: 
			- "puerto_host_n:puerto_imagen_n"
		environment:
			- VARIABLE_ENTORNO_1=valor_1
			- VARIABLE_ENTORNO_2=valor_2
```

## Ejecución

### Creación automática



Crea los contenedores deseados y las imágenes necesarias en base al archivo `docker-compose.yml`. 
```bash
docker compose up
docker-compose up		# versiones antiguas de Docker
```

También crea las redes necesarias por defecto.
Una vez terminado los hace funcionar. 
Puede interrumpirse haciendo  ++ctrl+c++ .

### Eliminación automática


Eliminar los contenedores diseñados con el archivo `docker-compose.yml`:
```bash
docker compose down
docker-compose down		# versiones antiguas de Docker
```
También elimina las redes comunes entre contenedores.




## Rama desarrollo

Es posible crear dos archivos alternos de configuración para crear una rama de desarrollo (*development*) de los contenedores sin afectar la rama que ya está operando. 


## Sintaxis

Para esto se crean dos archivos adicionales llamados `Dockerfile.dev` y `docker-compose-dev.yml`.
Este segundo archivo debe apuntar al primero, esto se hace añadiendo las siguientes lineas:
```yaml hl_lines="5-6"
#(dentro del archivo docker-compose-dev.yml)
version:  “numero_version”
services:
	contenedor_1:
		context:  .  	# ruta relativa al archivo .dev
		dockerfile: Dockerfile.dev  #se carga la plantilla de desarrollo
		ports:
		  #...
```

## Ejecución

Para operar con estos archivos se usa la opción `#!bash -f `del comando `#!bash docker compose`:

### Creación automática


```bash
docker compose -f docker-compose-dev.yml up
```


### Eliminación automática (PROBARR)


```bash
docker compose -f docker-compose-dev.yml  down
```





## PODMANNNNNN

Podman no trae incluida las herramientas para trabajar con archivos `docker-compose.yml`.
Sin embargo, existen paquetes adicionales qu







[Linux Config.org - How to use docker-compose with Podman on Linux](https://linuxconfig.org/how-to-use-docker-compose-with-podman-on-linux)