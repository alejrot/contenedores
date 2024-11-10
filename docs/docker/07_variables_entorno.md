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
  - MongoDB
---

# Variables de Entorno

Las variables de entorno se agregan al contenedor durante su creación, mediante la opción `-e`: 

=== "Docker"

    ```bash title="Crear contenedor - con variable entorno"
    docker create -e VARIABLE=VALOR  nombre_imagen
    ```

=== "Podman" 

    ```bash title="Crear contenedor - con variable entorno"
    podman create -e VARIABLE=VALOR  nombre_imagen
    ```

De usarse varias variables de entorno se usa la opción `-e` para separlas:


=== "Docker"

    ```bash title="Crear contenedor - con varias variables entorno"
    docker create -e VARIABLE_1=VALOR_1 -e VARIABLE_2=VALOR_2 nombre_imagen
    ```

=== "Podman" 

    ```bash title="Crear contenedor - con varias variables entorno"
    podman create -e VARIABLE_1=VALOR_1 -e VARIABLE_2=VALOR_2 nombre_imagen
    ```


Las variables de entorno requeridas por cada imagen
se indican en la página proveedora dede la descarga. 


!!! tip "Múltiples lineas"

    Para repartir las instrucciones de la terminal en varios renglones 
    se recurre al uso de **barras invertidas** (`\`).

    === "Docker"

        ```bash title="Crear contenedor - con varias variables entorno"
        docker create \
        -e VARIABLE_1=VALOR_1 \
        -e VARIABLE_2=VALOR_2 \ 
        nombre_imagen
        ```

    === "Podman" 

        ```bash title="Crear contenedor - con varias variables entorno"
        podman create \
        -e VARIABLE_1=VALOR_1 \
        -e VARIABLE_2=VALOR_2 \ 
        nombre_imagen
        ```

!!! example "MongoDB: usuario y contraseña"

    En el ejemplo se hace un contenedor de MongoDB (base de datos no relacional) 
    al que se le asigna nombre de usuario y contraseña internos para su consulta:

    === "Docker"

        ```bash title="MongoDB - con usuario y contraseña" hl_lines="2 3"
        docker create  \
        -e MONGO_INITDB_ROOT_USERNAME=mongoadmin \
        -e MONGO_INITDB_ROOT_PASSWORD=secret \
        --name mongo_admin  mongo 
        ```

    === "Podman" 

        ```bash title="MongoDB - con usuario y contraseña" hl_lines="2 3"
        podman create  \
        -e MONGO_INITDB_ROOT_USERNAME=mongoadmin \
        -e MONGO_INITDB_ROOT_PASSWORD=secret \
        --name mongo_admin  mongo 
        ```
    

