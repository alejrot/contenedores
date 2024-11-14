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


Las variables de entorno del contenedor se agregan al contenedor durante su creación, mediante la opción `-e`. 


## Valores fijos

La primera opción es asignar los valores a las variables de entorno durante la orden de creación del contenedor:

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


!!! tip "Múltiples lineas de la teminal"

    Para repartir las instrucciones de la terminal en varios renglones caa una
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



!!! example "Ejemplo: MongoDB con usuario y contraseña"

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


## Variables


Los valores necesarios se pueden pasar creando variables desde la terminal, antes de crear el contenedor.

```bash title="Variables - crear"
# creacion 
VARIABLE=valor
```

entonces el valor se asigna anteponiendo el símbolo `#!bash $`a cada variable:

=== "Docker"

    ```bash title="Crear contenedor - con variable "
    docker create -e VARIABLE=$VARIABLE  nombre_imagen
    ```

=== "Podman" 

    ```bash title="Crear contenedor - con variable"
    podman create -e VARIABLE=$VARIABLE  nombre_imagen
    ```


El valor de las variables se verifica con el comando `#!bash echo`

```bash title="Variables - consultar"
# consulta
echo $VARIABLE
echo ${VARIABLE}
```



## Variables de entorno


Con el uso de variables de entorno se evita el tener que asignar valor a cada variable del contenedor.

Para crear las variables de entorno en Bash se usa el comando `#!bash export`:

```bash title="Variables de entorno - crear"
# creacion
export VARIABLE_1=valor_1
export VARIABLE_2=valor_2
```

y al crear el contenedor ya no es necesario hacer la asignación, sino que con enumerar las variables alcanza:

=== "Docker"

    ```bash title="Crear contenedor - con variables de entorno"
    docker create -e VARIABLE_1 -e VARIABLE_2  nombre_imagen
    ```

=== "Podman" 

    ```bash title="Crear contenedor - con variables de entorno"
    podman create -e VARIABLE_1 -e VARIABLE_2  nombre_imagen
    ```



## Archivos `.env`


Las variables de entorno que necesita el contenedor se pueden guardar en forma de archivo de texto con la extensión `.env`:


```env
VARIABLE_1=valor_1 
VARIABLE_2=valor_2 
```

Cada línea de este archivo representa una variable de entorno.

Gracias a la opción `--env-file` las variables de entorno se pueden adjuntar todas juntas indicando el nombre de archivo:

=== "Docker"

    ```bash title="Crear contenedor - con variable entorno"
    docker create --env-file variables.env  nombre_imagen
    ```

=== "Podman" 

    ```bash title="Crear contenedor - con variable entorno"
    podman create --env-file variables.env  nombre_imagen
    ```


    



## Referencias

[Docker Docs - Set environment variables within your container's environment](https://docs.docker.com/compose/how-tos/environment-variables/set-environment-variables/#use-the-environment-attribute)