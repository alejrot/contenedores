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


## Crear 

=== "Docker"

	```bash title="Volumen - crear"
	docker volume create nombre_volumen
	```

=== "Podman" 

	```bash title="Volumen - crear"
	podman volume create nombre_volumen
	```

## Listar 

La lista de los volúmenes de usuario se obtiene con el comando `list`:

=== "Docker"

	```bash title="Listado de volumenes"
	docker volume list
	```

=== "Podman" 

	```bash title="Listado de volumenes"
	podman volume list
	```

## Inspeccionar

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


## Borrar

Los volumenes se pueden eliminar con ayuda del comando `rm`: 

=== "Docker"
	```bash title="Listado de volumenes"
	docker volume rm nombre_volumen
	```
=== "Podman" 
	```bash title="Listado de volumenes"
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






## Referencias

[RedHat.com - Compartiendo archivos entre dos contenedores](https://docs.redhat.com/es/documentation/red_hat_enterprise_linux/8/html/building_running_and_managing_containers/sharing-files-between-two-containers_building-running-and-managing-containers)