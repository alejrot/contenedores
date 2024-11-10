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
  - MySQL
  - PostgreSQL
#   - MariaDB
  - MongoDB
---



# Volumes con Docker Compose

<!-- # Volumes -->
<!-- 
Definir los volúmenes ( *volumes* ) previene la eliminación de datos internos de los contenedores cuando se eliminan estos últimos. 
Los datos se mantienen fuera del contenedor, en el directorio del sistema anfitrión que se montó (es decir el volumen elegido). 
Estos datos pueden ser registros del funcionamiento del contenedor, bases de datos creadas en la ejecución, rutinas de lenguajes interpretados por el contenedor que se requiere poder actualizar desde fuera, etc. -->


<!-- 
### Tipos de volúmenes:

- anonimos: solo indica la ruta de origen a montar. Docker guarda donde le parece. No es referenciable desde afuera.
- De anfitrión o host:se indica qué carpeta montar y adonde montarla
- Nombrado:  similar al anónimo puede ser referenciado por múltiples contenedores 
-->



### Sintaxis

Los volúmenes se definen y configuran dentro del archivo `docker-compose.yml`:

```yaml
version:  “numero_version”

services:

	contenedor_1:
		build:  ruta_Dockerfile 
		ports:
			# lista de mapeos de peurtos
			- “puerto_host_1:puerto_imagen_1”
			- “puerto_host_2:puerto_imagen_2” 
			# respetar las comillas dobles para rodear los pares

		links:
 			- contenedor_2 		#contenedor destino

	contenedor_2:
		image: imagen_2		# (Imagen preexistente)
		ports: 
			- "puerto_host_n:puerto_imagen_n"
		environment:
			- VARIABLE_ENTORNO_1=valor_1
			- VARIABLE_ENTORNO_2=valor_2
		volumes:
			# lista de volumenes accesibles 
			- contenedor_2-data:  ruta_data_2

volumes:
	# nombre para el volumen 
	contenedor_2-data:	# queda vacío

```

**TIP: rutas para bases de datos**

Los gestores de bases de datos tienen ciertas rutas predefinidas para guardar la información.
En los sistemas Linux estas rutas son:

- MongodB:`/data/db`
- MySQL `/var/lib/mysql`
- PostgreSQL: `/var/lib/postgresql/data`

Dado que las imágenes de Docker son basadas en distribuciones Linux, estas mismas rutas son las usadas adentro del contenedor.
