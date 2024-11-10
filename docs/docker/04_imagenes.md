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

<!-- 
=== "Docker"
=== "Podman"
 -->



# Imágenes


Las imágenes son los componentes de software encapsulados,
los cuales pueden incorporar intérpretes de lenguajes, bibliotecas, frameworks, etc.


### Listado


Indica qué imágenes están descargadas en la PC y sus características básicas: versión, identificador, espacio ocupado, etc.

=== "Docker"

    ```bash title="Listar imágenes locales"
    docker images
    ```

=== "Podman"

    ```bash title="Listar imágenes locales"
    podman images
    ```




### Descarga

Descarga la última versión disponible de la imagen indicada desde DockerHub 
y la etiqueta como `latest`:

=== "Docker"

    ```bash title="Descargar imagen - Última release"
    docker pull nombre_imagen
    ```

=== "Podman"

    ```bash title="Descargar imagen - Última release"
    podman pull nombre_imagen
    ```

Descarga la versión indicada de la imagen  y la etiqueta numerada.

=== "Docker"

    ```bash title="Descargar imagen - version específica"
    docker pull nombre_imagen:version
    ```
=== "Podman"

    ```bash title="Descargar imagen - version específica"
    podman pull nombre_imagen:numero_version
    ```

Si no se indica la versión se descarga la más reciente (`latest`):




!!! warning "Opciones de imagen"

    Cada imagen puede tener distintas opciones de configuración. Por ello hay que chequear también las opciones que se requieren para configurar los futuros contenedores basados en estas imágenes. 



!!! info "DockerHub"

    En [DockerHub](https://hub.docker.com/) se puede consultar qué imágenes hay disponibles e indica el comando completo para descargarlas. 



!!! info "Fuentes de imágenes"

    - [Docker](docker.io/)
    - [Fedora Project](registry.fedoraproject.org/)
    - [Red Hat](registry.access.redhat.com/) (Es de pago)
    - [Quay](quay.io/)


**Importante:** 
hay que chequear también los comandos para configurar los futuros contenedores basados en estas imágenes.



### Borrado

Elimina del disco la imagen especificada:

=== "Docker"

    ```bash
    docker image rm nombre-imagen:numero-version
    ```
=== "Podman"

    ```bash title="Eliminar imagen local"
    podman image rm nombre_imagen:numero_version
    ```

