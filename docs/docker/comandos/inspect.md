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
#   - MySQL
#   - PostgreSQL
#   - MariaDB
#   - MongoDB
---







# Inspeccionar metadatos

Los objetos del gestor de contenedor (imagenes, contenedores, redes, volumenes, etc) traen adjuntos los metadatos, que son una colección de datos informativos sobre dichos objetos: version, nombre, ubicación en el disco, etc.


## Uso básico

Los metadatos se leen con el comando `inspect`:

=== "Docker"

    ```bash  title="inspeccionar"
    docker inspect objeto
    ```

=== "Podman" 

    ```bash  title="inspeccionar"
    podman inspect objeto
    ```

La metadata se devuelve completa como un objeto JSON, es decir como conjunto de pares clave - valor. Dichas claves pueden venir anidadas: algunas claves pueden ser valores de otras claves.


## Distinción por tipos

Varios objetos de distinto tipo pueden compartir nombre. 
Por ejemplo: un contenedor con un servidor interno puede llamarse `server_8000` y el mismo contenedor podría tener acceso a un volumen llamado `server_8000`.

Para poder diferenciar unos objetos de otros se puede recurr a la opción `--type`:

=== "Docker"

    ```bash  title="inspeccionar - por tipo"
    docker inspect --type=container server_8000
    docker inspect --type=volume    server_8000
    ```
=== "Podman" 

    ```bash  title="inspeccionar - por tipo"
    podman inspect --type=container server_8000
    podman inspect --type=volume    server_8000
    ```



## Lectura de parámetros


Si se busca solamente un parámetro particular se usa la opción `-f` (`--format` ):


=== "Docker"

    ```bash  title="inspeccionar - parametro IP"
    docker inspect  -f '{{.NetworkSettings.IPAddress}}' nombre_contenedor
    docker inspect  --format='{{.NetworkSettings.IPAddress}}' nombre_contenedor
    ```

=== "Podman" 

    ```bash  title="inspeccionar - parametro IP"
    podman inspect  -f '{{.NetworkSettings.IPAddress}}' nombre_contenedor
    podman inspect  --format='{{.NetworkSettings.IPAddress}}' nombre_contenedor
    ```

En este ejemplo se consulta la clave `NetworkSettings` y entre sus valores se busca la clave `IPAddress`. 


## Tamaño


En el caso específico de los contenedores se puede agregar la opción `--size`:


=== "Docker"

    ```bash  title="inspeccionar - dimensiones agregadas"
    docker inspect --size nombre_contenedor
    ```

=== "Podman" 

    ```bash  title="inspeccionar - dimensiones agregadas"
    podman inspect --size nombre_contenedor
    ```


Esta opción agrega dos nuevos campos de datos:

- `SizeRootFs`: espacio total (en bytes) del contenedor
- `SizeRw` : espacio de los archivos creados o modificados del contenedor. 


=== "Docker"

    ```bash  title="inspeccionar - espacio total"
    docker inspect --size nombre_contenedor -f '{{ .SizeRootFs }}'
    ```

    ```bash  title="inspeccionar - espacio de cambios"
    docker inspect --size nombre_contenedor -f '{{ .SizeRw }}'  
    ```

=== "Podman" 

    ```bash  title="inspeccionar - espacio total"
    podman inspect --size nombre_contenedor -f '{{ .SizeRootFs }}'
    ```

    ```bash  title="inspeccionar - espacio de cambios"
    podman inspect --size nombre_contenedor -f '{{ .SizeRw }}'  
    ```


## Referencias


[Docker Docs - docker inspect](https://docs.docker.com/reference/cli/docker/inspect/#get-an-instances-ip-address)