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
  - Bash
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


Las variables de entorno se agregan al contenedor durante su creación, mediante la opción `-e`. 


=== "Docker"

    ```bash title="Crear contenedor - con variable entorno"
    docker create -e VARIABLE=valor  nombre_imagen
    ```

=== "Podman" 

    ```bash title="Crear contenedor - con variable entorno"
    podman create -e VARIABLE=valor  nombre_imagen
    ```

De usarse varias variables de entorno se usa la opción `-e` para separlas:


=== "Docker"

    ```bash title="Crear contenedor - con varias variables entorno"
    docker create -e VARIABLE_1=valor_1 -e VARIABLE_2=valor_2 nombre_imagen
    ```

=== "Podman" 

    ```bash title="Crear contenedor - con varias variables entorno"
    podman create -e VARIABLE_1=valor_1 -e VARIABLE_2=valor_2 nombre_imagen
    ```


Las variables de entorno requeridas por cada imagen
se indican en la página proveedora de la descarga. 


Los valores necesarios se pueden pasar tambińe usando variables de entorno desde la terminal.

Por ejemplo, para crear una variable de entorno en Bash se usa el comando `#!bash export`:


```bash title="Variables de entorno - crear"
# creacion
export VARIABLE_ENTORNO=valor
```

entonces el valor se asigna anteponiendo el símbolo `#!bash $`a la variable:

=== "Docker"

    ```bash title="Crear contenedor - con variable entorno"
    docker create -e VARIABLE=$VARIABLE_ENTORNO  nombre_imagen
    ```

=== "Podman" 

    ```bash title="Crear contenedor - con variable entorno"
    podman create -e VARIABLE=$VARIABLE_ENTORNO  nombre_imagen
    ```


El valor de las variables se verifica con el comando `#!bash echo`

```bash title="Variables de entorno - consultar"
# consulta
echo $VARIABLE_ENTORNO
echo ${VARIABLE_ENTORNO}
```



!!! tip "Múltiples lineas"

    Para repartir las instrucciones de la terminal en varios renglones 
    se recurre al uso de **barras invertidas** (`\`).

    === "Docker"

        ```bash title="Crear contenedor - con varias variables entorno"
        docker create \
        -e VARIABLE_1=valor_1 \
        -e VARIABLE_2=valor_2 \ 
        nombre_imagen
        ```

    === "Podman" 

        ```bash title="Crear contenedor - con varias variables entorno"
        podman create \
        -e VARIABLE_1=valor_1 \
        -e VARIABLE_2=valor_2 \ 
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
    

