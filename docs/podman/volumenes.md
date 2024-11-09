---
tags:
#   - HTML5
  # - JavaScript
  # - CSS
#   - YAML
#   - MkDocs
#   - Python
#   - Docker
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

# Volúmenes


Los volúmenes son directorios del sistema anfitrión a los cuales los contenedores pueden acceder para leer o escribir archivos. Los volumenes pueden ser compartidos por varios contenedores y además facilitan las tareas de respaldo de datos y de migración. Además, los volumenes no aumentan el tamaño de los contenedores por ser externos a los mismos.


## Crear 

```bash title="Volumen - crear"
podman volume create nombre_volumen
```

## Inspeccionar

```bash title="Volumen - inspeccionar"
podman volume inspect nombre_volumen 
```

El punto de montaje es la ruta donde el volumen almacenará los datos. Éste se consulta con la opción `--format {{.Mountpoint}}` :


```bash title="Volumen - punto de montaje"
podman volume inspect nombre_volumen --format {{.Mountpoint}}
```

En el ejemplo, el punto de montaje queda:
`/home/USUARIO/.local/share/containers/storage/volumes/nombre_volumen/_data`

En sistemas Linux, la ruta habitual de almacenamiento de los volúmenes es:

`/home/USUARIO/.local/share/containers/storage/volumes/`


## Listar 

La lista de los volúmenes de usaurio se obtiene con el comando `list`:

```bash title="Listado de volumenes"
podman volume  list
```


## Borrar

Los volumenes se pueden eliminar con ayuda del comando `rm`: 

```bash title="Listado de volumenes"
podman volume rm nombre_volumen
```



!!! info "Volumenes del sistema"

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