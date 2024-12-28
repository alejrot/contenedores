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


# Editar Imágenes


A menudo es necesario modificar las imágenes preexistentes para poder crear las nuevas aplicaciones. 
Para poder crearlas es indispensable crear primero los archivos de configuración, conocidos típicamente como Dockerfiles.




## Crear imágenes - `build`

La imagen nueva se construye con el comando `build`:

=== "Docker"

    ```bash title="Crear imagen "
    docker build  ruta_Dockerfile
    ```
    
=== "Podman" 

    ```bash title="Crear imagen "  
    podman build  ruta_Dockerfile
    ```

Con él se construye una imagen a partir del archivo Dockerfile
disponible en la ruta indicada,
la cual puede ser absoluta o relativa.
El resultado es una imagen nueva sin nombre pero con un identificador único.


Para facilitar el manejo de las imágenes se les asigna un nombre y una etiqueta de versión:

=== "Docker"

    ```bash title="Crear imagen - con nombre y versión"
    docker build -t nombre_imagen:tag_version  ruta_Dockerfile
    ```
    
=== "Podman" 

    ```bash title="Crear imagen - con nombre y versión"  
    docker build -t nombre_imagen:tag_version  ruta_Dockerfile
    ```


Si la terminal ya está ubicada en el directorio raíz de la aplicación hay que poner un punto: 


=== "Docker"

    ```bash title="Crear imagen - directorio actual" 
    cd ruta_Dockerfile
    docker build -t nombre_imagen:numero_version  .
    ```

=== "Podman" 

    ```bash title="Crear imagen - directorio actual" 
    cd ruta_Dockerfile
    podman build -t nombre_imagen:numero_version  .
    ```


La imagen creada 
aparecerá incluida en la lista de imágenes locales y
podrá ser utilizada como cualquier imagen prefabricada 
para crear nuevos contenedores.




Por ejemplo, para crear una imagen de nombre `miapp` y su tag de versión es `latest` el comando será:

=== "Docker" 

    ```bash  
    docker build -t miapp:latest .
    ```

=== "Podman" 

    ```bash  
    podman build -t miapp:latest .
    ```

## Nombrar imagenes - `tag`

Las imágenes ya creadas se renombran fácilmente con ayuda del comando `tag`:

=== "Docker"

    ```bash
    docker tag nombre_actual nuevo_nombre:tag_version   # por nombre
    docker tag id_imagen     nuevo_nombre:tag_version   # por ID
    ```
=== "Podman" 

    ```bash
    podman tag nombre_actual nuevo_nombre:tag_version   # por nombre
    podman tag id_imagen     nuevo_nombre:tag_version   # por ID
    ```

Debe tenerse en cuenta que una misma imagen puede tener varias etiquetas en simultáneo. 





## Compartir imagen

Nos identificamos para que el servidor nos permita subir la imagen a nuestro nombre. Para ello usamos el comando `login`:

=== "Docker"

    ```bash title="Login - Docker Hub"
    docker login
    ```

=== "Podman" 

    ```bash title="Login - Red Hat"
    podman login
    ```

Nótese que el destino predefinido para las imágenes de Docker es Docker Hub, 
en tanto que para Podman el destino es Red Hat.


Ordenamos el envío de nuestra imagen al servidor para publicarla, 
usando el comando `push`:

=== "Docker"

    ```bash title="Publicar"
    docker push mi_imagen:tag_version
    ```

=== "Podman" 

    ```bash title="Publicar"
    podman push mi_imagen:tag_version
    ```

!!! info "Docker Hub"

    Docker Hub da alojamiento gratuito a las imágenes
    que sean compartidas públicamente por los usuarios.

