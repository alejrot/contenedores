---
tags:
#   - HTML5
  # - JavaScript
  # - CSS
#   - YAML
#   - MkDocs
  - Python
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
#   - MongoDB
---



# Archivo Dockerfile

El archivo Dockerfile nos permite crear nuevas imágenes en base a una preexistente donde se incluyan nuevos comandos y aplicaciones.
Este archivo debe mantener el nombre `Dockerfile` para ser utilizado y no tiene extensión de archivo.

### Sintaxis

La sintaxis del `Dockerfile` es la siguiente:

```Dockerfile title="Dockerfile - sintaxis genérica"
FROM imagen_base:version

RUN mkdir -p "ruta_ejecutables"

RUN comandos_instalacion 

WORKDIR "ruta_entorno"

COPY "ruta_host" "ruta_interna"

EXPOSE numero_puerto

CMD [ "comando" , "ruta_ejecutable" ]
```

La función resumida de cada comando es la siguiente:

|Cláusula| Uso|
|:---|:---|
|`FROM`| Elige una imagen preexistente que servirá de referencia|
|`RUN`| Ejecuta comandos en Bash para crear directorios e instalar software |
|`COPY`| Copia archivos del *host* a la futura imagen|
|`WORKDIR`| Cambia la ruta interna del entorno de ejecución |
|`EXPOSE`| Especifica el nº de puerto para conectar con la imagen|
|`CMD`| Ordena la ruta y el nombre del comando a ejecutar |

No es obligatorio especificar todos los campos,
sino que estos se indican o no 
según se necesite.



!!! tip "Directorio para ejecutables"

    Una ruta habitual para ubicar los ejecutables dentro de la imagen es `/home/app`. 
    Este directorio debe ser creado para su uso.



<!-- 
El comando `RUN` permite instalar aplicaciones dentro de la imagen de ser necesario.  -->
<!-- 
Es posible indicar un directorio de funcionamiento con el comando `WORKDIR`, de este modo la sintaxis quedaría así:

```Dockerfile
[...]

WORKDIR ruta_destino

[...]

CMD [comando , ejecutable]
```
-->



!!! example "Ejemplo: ejecutables en Python"

    La imagen oficial de [Python 3](https://hub.docker.com/_/python) propone la siguiente plantilla para crear imágenes derivadas:

    ```Dockerfile
    FROM python:3

    WORKDIR /usr/src/app

    COPY requirements.txt ./
    RUN pip install --no-cache-dir -r requirements.txt

    COPY . .

    CMD [ "python", "./script_python.py" ]
    ```
    donde `requirements.txt` es el archivo con la lista de todos los paquetes de Python a incluir adentro de la imagen.



### Crear imágenes modificadas

La imagen se construye con el comando `build`:

=== "Docker"

    ```bash
    docker build -t nombre_imagen:numero_version  ruta_Dockerfile
    ```
    
=== "Podman" 

    ```bash   
    docker build -t nombre_imagen:numero_version  ruta_Dockerfile
    ```

Con él se construye una imagen a partir del archivo Dockerfile
disponible en la ruta indicada,
la cual puede ser absoluta o relativa.

Si la terminal ya está ubicada en el directorio raíz de la aplicación hay que poner un punto: 


=== "Docker"

    ```bash
    cd ruta_Dockerfile
    docker build -t nombre_imagen:numero_version  .
    ```

=== "Podman" 

    ```bash
    cd ruta_Dockerfile
    podman build -t nombre_imagen:numero_version  .
    ```


La imagen creada podrá ser utilizada como cualquier imagen prefabricada 
para crear nuevos contenedores.



Ejemplo: `docker build -t miapp:1 .`



