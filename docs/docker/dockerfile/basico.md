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

## Introducción

El archivo Dockerfile nos permite crear nuevas imágenes en base a una preexistente donde se incluyan nuevos comandos y aplicaciones.
Este archivo suele llamarse `Dockerfile` para ser utilizado y no tiene extensión de archivo.



!!! info "Containerfile vs Dockerfile"

    `Dockerfile` es el nombre más habitual para el archivo de creación de imagen debido a la gran popularidad de Docker; 
    sin embargo la norma es el uso del nombre genérico `Containerfile`.



## Sintaxis

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

|Cláusula| Argumentos | Uso|
|:---|:---:|:---|
|`FROM`| `nombre_imagen:tag` | Elige una imagen preexistente que servirá de referencia|
|`RUN`| `comando_completo` | Ejecuta comandos en Bash para crear directorios e instalar software |
|`COPY`|`ruta_host  ruta_interna`| Copia archivos del *host* a la futura imagen|
|`WORKDIR`|`ruta_trabajo`| Cambia la ruta interna del entorno de ejecución |
|`EXPOSE`|`numero_puerto`| Especifica el Nº de puerto para conectar con la imagen (informativo)|
|`CMD`|`[comando, rutina_interna]`| Llama al ejecutable interno y le especifica una rutina interna como argumento |
|`ENTRYPOINT`|`[comando]`| Llama al ejecutable interno pero exigiendo uno o varios argumentos externos|
|`ENV`| `nombre_variable  valor`| Define variables de entorno para la imagen |
|`USER`|`usuario_o_id`| Define el usuario por su nombre o por su *id* |


No es obligatorio especificar todos los campos,
sino que estos se indican o no 
según se necesite.


Por ejemplo, `CMD` y `ENTRYPOINT` tienen usos alternativos:

 <div class="grid" markdown>

```dockerfile title="Rutina única - CMD"
# Servidor Python 
CMD ["python", "/usr/src/myapp/server.py"]
```

```dockerfile  title="Rutina externa - ENTRYPOINT"
# Intérprete Python encapsulado 
ENTRYPOINT ["python"]
```
 </div>

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



## Referencias

[Docker Docs - Writing a Dockerfile](https://docs.docker.com/get-started/docker-concepts/building-images/writing-a-dockerfile/)