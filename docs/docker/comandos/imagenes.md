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


# Imágenes Preconstruidas


Las imágenes son los componentes de software encapsulados,
los cuales pueden incorporar intérpretes de lenguajes, bibliotecas, frameworks, etc.

En este capítulo se explican solamente los comandos requeridos para trabajar con imágenes prefabricadas y disponibles online.

!!! info "DockerHub"

    En [DockerHub](https://hub.docker.com/) se puede consultar qué imágenes hay disponibles e indica el comando completo para descargarlas. 


!!! info "Fuentes de imágenes"

    - [DockerHub](https://hub.docker.com/)
    - [Fedora Project](https://registry.fedoraproject.org/)
    - [Red Hat](https://registry.access.redhat.com/) (Es de pago)
    - [Quay](https://quay.io/)

### Listado


El comando `images` indica qué imágenes están descargadas en la PC 
y sus características básicas: versión, identificador, espacio ocupado, etc.

=== "Docker"

    ```bash title="Listar imágenes locales"
    docker images
    ```

=== "Podman"

    ```bash title="Listar imágenes locales"
    podman images
    ```




### Descarga


El comando `pull` descarga la última versión disponible 
de la imagen indicada desde DockerHub 
y la etiqueta como `latest`:

=== "Docker"

    ```bash title="Descargar imagen - Última release"
    docker pull nombre_imagen
    ```

=== "Podman"

    ```bash title="Descargar imagen - Última release"
    podman pull nombre_imagen
    ```
<!-- 
Descarga la versión indicada de la imagen y la etiqueta numerada.
-->

Se puede especificar una versión de la imagen para la descarga a continuación de su nombre:

=== "Docker"

    ```bash title="Descargar imagen - version específica"
    docker pull nombre_imagen:version
    ```
=== "Podman"

    ```bash title="Descargar imagen - version específica"
    podman pull nombre_imagen:numero_version
    ```

Si no se indica la versión se descarga la más reciente (`latest`).
Otras versiones habituales son `stable`, `devel`, `latest`, `nightly`, etc.
La información de dichas versiones debe aparecer en la página de descarga de la imagen elegida.


!!! warning "Opciones de imagen"

    Cada imagen puede tener distintas opciones y variables de configuración, 
    los cuales habitualmente se indican en la página  proveedora dede la descarga.

    Por ello hay que chequear también los parámetros que se requieren para configurar los futuros contenedores basados en estas imágenes.  
   

<!-- 
**Importante:** 
hay que chequear también los comandos para configurar los futuros contenedores basados en estas imágenes.
 -->


### Borrado

Las imágenes se eliminan del disco con el comando `rm`:

=== "Docker"

    ```bash title="Eliminar imagen local"
    docker image rm nombre_imagen:numero_version
    ```
=== "Podman"

    ```bash title="Eliminar imagen local"
    podman image rm nombre_imagen:numero_version
    ```

