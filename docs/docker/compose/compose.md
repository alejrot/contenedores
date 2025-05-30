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
#   - MongoDB
---





# Compose

## Introducción

El comando Compose habilita usar una plantilla para facilitar  la creación de los contenedores y su interconexión. 

El comando `docker compose` viene integrado por defecto en Docker;
en cambio Podman no lo trae incorporado sino que debe ser instalado y habilitado como extensión 
y se puede invocar tanto por `docker-compose` como por `podman compose` 

El comando presupone la existencia de un archivo de configuración, 
tal como se explica a continuación.

## Archivos Compose

El archivo de configuración típico se llama `docker-compose.yml`.  

Dentro del archivo debe respetarse estrictamente la indentación sino no funciona.
También hay que respetar las comillas para los números de puerto y los guiones donde se indican. 
Los comentarios son precedidos del carácter `#`.
En caso de indicarse,
Docker Compose aprovecha el archivo Dockerfile por defecto para completar la configuración del proyecto.

## Sintaxis básica

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
			# (respetar las comillas dobles para los puertos)

		links:
			 - contenedor_2 	# contenedor destino

	contenedor_2:
		image: imagen_2		# (Imagen preexistente)
		ports: 
			- "puerto_host_n:puerto_imagen_n"
		environment:
			- VARIABLE_ENTORNO_1=valor_1
			- VARIABLE_ENTORNO_2=valor_2
		volumes:
			# lista de volumenes accesibles 
			- nombre_volumen: ruta_montaje_interna 

volumes:
	# nombre para el volumen 
	nombre_volumen: 
```
!!! info "version"

	La etiqueta `version` está obsoleta y es ignorada.


!!! warning "`link` vs `depends_on`"

	La opción `link`, usada para comunicar un servicio/contenedor con otro, está obsoleta y el Compose de Podman ya no la ejecuta arrojando un error.
	Su reemplazo es la opción `depends_on`.

	```yaml
	links:
		- contenedor_2 		#contenedor destino
	```

	```yaml
	depends_on:
		- contenedor_2 		#contenedor destino
	```

	Más info:
	[Stack Overflow - Difference between links and depends_on in docker_compose.yml](https://stackoverflow.com/questions/35832095/difference-between-links-and-depends-on-in-docker-compose-yml)



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



https://www.redhat.com/sysadmin/podman-docker-compose



## REFERENCIATUS!!!


https://compose-spec.io

https://github.com/compose-spec/compose-spec/blob/main/spec.md


https://docs.docker.com/reference/compose-file/


## NETWORKING!!!


[Newtworking](https://docs.docker.com/compose/how-tos/networking/)


## SECRETOSSS!!!!

[Docker Docs - Secrets ](https://docs.docker.com/compose/how-tos/use-secrets/)

[Secrets top-level elements](https://docs.docker.com/reference/compose-file/secrets/)

## `docker compose` en Podman



instalar `podman-docker` y `docker-compose`


Arrancar servicio Podman:

    sudo systemctl start podman.socket

Verificar conexion:

    sudo curl -H "Content-Type: application/json" --unix-socket /var/run/docker.sock http://localhost/_ping




## Referencias


[Red Hat - Using Podman and Docker Compose](https://www.redhat.com/sysadmin/podman-docker-compose)

[Linux Config.org - How to use docker-compose with Podman on Linux](https://linuxconfig.org/how-to-use-docker-compose-with-podman-on-linux)





[Docker Docs - Volumes top-level element](https://docs.docker.com/reference/compose-file/volumes/)