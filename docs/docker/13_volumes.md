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
  - MariaDB
---

# Volumenes


Los volúmenes o *volumes* son directorios del sistema anfitrión a los cuales los contenedores pueden acceder para leer o escribir archivos de datos. 
Estos datos pueden ser registros del funcionamiento del contenedor, bases de datos creadas en la ejecución, rutinas de lenguajes interpretados por el contenedor que se requiere poder actualizar desde fuera, etc.
El uso de volúmenes permite mantener los datos afuera del contenedor,
previniendo su borrado cuando se eliminan los contenedores. 


## Tipos de volúmenes

Hay varios tipos de volumenes en base a su creación:

- **Anónimos:**
en éstos solo se indica la ruta de origen a montar.
No son referenciables desde otros contenedores. 
El gestor de contenedores elige la ubicación automáticamente. 
- **Nombrados:**
son similares a los volúmenes anónimos pero pueden ser referenciados por múltiples contenedores gracias a su nombre, 
permitiendo su compartimento y reutilización.
- **De anfitrión o host:**
en estos volúmenes se indica qué carpeta montar y adonde montarla.


## Gestionar volumenes

### Crear 

=== "Docker"

	```bash title="Volumen - crear"
	docker volume create nombre_volumen
	```

=== "Podman" 

	```bash title="Volumen - crear"
	podman volume create nombre_volumen
	```

### Listar 

La lista de los volúmenes de usuario se obtiene con el comando `list`:

=== "Docker"

	```bash title="Listado de volumenes"
	docker volume list
	```

=== "Podman" 

	```bash title="Listado de volumenes"
	podman volume list
	```

### Inspeccionar

La información del volumen elegido se obtiene con el comando `inspect`:

=== "Docker"

	```bash title="Volumen - inspeccionar"
	docker volume inspect nombre_volumen 
	```

=== "Podman" 

	```bash title="Volumen - inspeccionar"
	podman volume inspect nombre_volumen 
	```

El punto de montaje es la ruta donde el volumen almacenará los datos. Éste se consulta con la opción `--format {{.Mountpoint}}` :

=== "Docker"

	```bash title="Volumen - punto de montaje"
	docker volume inspect nombre_volumen --format {{.Mountpoint}}
	```

=== "Podman" 

	```bash title="Volumen - punto de montaje"
	podman volume inspect nombre_volumen --format {{.Mountpoint}}
	```

En el ejemplo, el punto de montaje queda:
`/home/USUARIO/.local/share/containers/storage/volumes/nombre_volumen/_data`


!!! info "Volumenes en Linux"

    En sistemas Linux, la ruta habitual de almacenamiento de los volúmenes es:

    `/home/USUARIO/.local/share/containers/storage/volumes/`


Más información sobre el [comando inspect](14_inspect.md).


### Borrar voluemn

Los volumenes se pueden eliminar uno a uno con ayuda del comando `rm`: 

=== "Docker"
	```bash title="Borrar volumen"
	docker volume rm nombre_volumen
	```
=== "Podman" 
	```bash title="Borrar volumen"
	podman volume rm nombre_volumen
	```



!!! info "Volumenes del sistema en Linux"

    Los volúmenes del sistema son invisibles para el usuario a menos que se recurra a la orden `sudo` para ejecutar los comandos:

    ```bash title="volumenes del sistema"
    sudo podman volume create nombre_volumen    # crear
    sudo podman volume inspect nombre_volumen   # inspeccionar
    sudo podman volume list                     # listar
    sudo podman volume rm  nombre_volumen       # borrar
    ```

    Los volumenes del sistema en Linux se guardan en la ruta:
    `/var/lib/containers/storage/volumes/`


### Borrar (no utilizados)


Con el comando `prune` se puede eliminar aquellos contenedores que no están siendo usados por ningun contenedor: 

=== "Docker"

	```bash	title="Borrar volumenes no usados"
	docker volume prune
	```

=== "Podman" 

	```bash	title="Borrar volumenes no usados"
	podman volume prune
	```

Este comando lista los volumenes susceptibles de ser borrados y pide confirmación antes de eliminarlos.


## Crear contenedores con volumenes

### Agregado de volumen

La forma más simple de configurar el agregado de volumenes es mediante la opción `-v`. 
Con ella se especifican el nombre del volumen 
y la ruta interna del contenedor sobre la que se debe montar el volumen.

=== "Docker"

	```bash title="montaje de volumen - solo lectura"
	docker create -v nombre_volumen=ruta_montaje_interna  nombre_imagen
	```


=== "Podman" 

	```bash title="montaje de volumen - solo lectura"
	podman create -v nombre_volumen=ruta_montaje_interna  nombre_imagen
	```


!!! tip "TIP: rutas para bases de datos"

	Los gestores de bases de datos tienen ciertas rutas predefinidas para guardar la información. 
	En los sistemas Linux estas rutas son:

	|Base de datos| ruta de montaje|
	|---|---|
	|**MySQL** |`/var/lib/mysql`|
	|**PostgreSQL**| `/var/lib/postgresql/data`|
	|**MongodB** |`/data/db`|

	Dado que las imágenes de Docker y Podman son basadas en distribuciones Linux, estas mismas rutas son las usadas adentro del contenedor.




### Sólo lectura


Los volumenes se pueden agregar a los contenedores con la opción `ro` (*read only*)


```bash	title="montaje de volumen - solo lectura"
podman create -v nombre_volumen=ruta_montaje_interna:ro  nombre_imagen
```



## Ejemplo aplicado: base de datos

En este ejemplo se crea una base de datos SQL con la [imagen oficial de MariaDB](https://hub.docker.com/_/mariadb).
Se agregan las siguientes configuraciones:

- la conexion se mantiene en el puerto `3306` (predefinido);
- acceso con usuario `root` (administrador) y contraseña `1234`;
- una base de datos vacía llamada `mi-db`;
- un volumen pesistente para la base de datos interna. 

Primero se exportan los valores de las variables de entorno:

```bash title="variables de entorno"
# variables de entorno
export MARIADB_ROOT_PASSWORD=1234
export MARIADB_DATABASE=mi-db
```


Luego se crea un volumen nombrado `mariadb-persistente`:

=== "Docker"

	```bash title="volumen"
	# creacion volmen
	docker volume create mariadb-persistente
	```
=== "Podman" 

	```bash title="volumen"
	# creacion volmen
	podman volume create mariadb-persistente 
	```


Y por último se crea el contenedor, con el nombre `mariadb-test`:

=== "Docker"

	```bash title="contenedor con volumen" hl_lines="4"
	docker create --name mariadb-test -p3306:3306 \
	-e MARIADB_DATABASE \
	-e MARIADB_ROOT_PASSWORD \
	-v mariadb-persistente:/var/lib/mysql \
	--replace  mariadb:latest  
	```

=== "Podman" 

	```bash title="contenedor con volumen" hl_lines="4"
	podman create --name mariadb-test -p3306:3306 \
	-e MARIADB_DATABASE \
	-e MARIADB_ROOT_PASSWORD \
	-v mariadb-persistente:/var/lib/mysql \
	--replace  mariadb:latest  
	```

Para poder verificar que el contenedor está bien configurado sólo hay que cargar los datos en un conector o programa *front* compatible con bases MariaDB y verificar que la conexión es exitosa.
Y para corroborar la persistencia de datos hay que:

- crear cambios (tablas, otras bases de datos, etc);
- borrar el contenedor
- crear el contenedor de nuevo.

Los datos agregados o modificados deben seguir allí.



## Referencias

[RedHat.com - Compartiendo archivos entre dos contenedores](https://docs.redhat.com/es/documentation/red_hat_enterprise_linux/8/html/building_running_and_managing_containers/sharing-files-between-two-containers_building-running-and-managing-containers)



[Docker Docs - Volumes](https://docs.docker.com/engine/storage/volumes/)