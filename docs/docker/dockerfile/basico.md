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

# Configurar imágenes



## Dockerfile

Las imágenes no se crean ni modifican directamente mediante el uso de comandos,
sino que se recurre al uso de un archivo de configuración especial. 
Este archivo suele llamarse `Dockerfile` 
y
nos permite configurar nuevas imágenes en base a una preexistente donde se incluyan nuevos comandos y aplicaciones.
El `Dockerfile` sirve de plantilla para el [comando build](build.md), el cual es el encargado de leer todas las configuraciones y volcarlas en una nueva imagen.


!!! info "Containerfile vs Dockerfile"

    `Dockerfile` es el nombre más habitual para el archivo de creación de imagen debido a la gran popularidad de Docker; 
    sin embargo la norma es el uso del nombre genérico `Containerfile`.



## Sintaxis

Los archivos `Dockerfile` suelen tener una sintaxis como la siguiente:

```Dockerfile title="Dockerfile - sintaxis genérica"
FROM imagen_base:version

RUN mkdir -p "ruta_ejecutables"

RUN comandos_instalacion 

WORKDIR "ruta_entorno"

COPY "ruta_host" "ruta_interna"

EXPOSE numero_puerto

CMD [ "comando" , "ruta_ejecutable" ]
```


## Cláusulas básicas

Aquí se ven los comandos más habituales:


### `FROM`

La cláusula `FROM` especifica la imagen que servirá de base para crear la nueva imagen. Típicamente es una imagen de alguna distribución de Linux 
(típicamente Ubuntu, Debian, Alpine) o alguna imagen ya derivada de estas, la cual puede incluir:

- Intérpretes y entornos de ejecución para lenguajes: Python, NodeJS, Golang, etc;
- frameworks específicos: Express, Flask, etc.
- Servidores web prearmados: Apache, NGINX, etc.

Esta cláusula es de uso obligatorio.


### `RUN`

La cláusula sirve para ordenar al kernel interno de la imagen la ejecución de comandos específicos como:

- Administrar el sistema de archivos interno: 
    - crear archivos y carpetas; 
    - mover elementos de ubicación; 
    - administrar permisos internos de ser necesario; 
    - etc.

- Descargar e instalar paquetes de la distribución;
- Usar el gestor de paquetes del lenguaje de programación a utilizar: PIP, NPM, etc.

Esta cláusula puede invocarse múltiples veces.


### `COPY`

Esta cláusula copia contenidos del sistema anfitrión al interior de la futura imagen:
rutinas, bibliotecas, ejecutables, etc.
De esta forma los contenedores podran tener acceso a los mismos. 

!!! info "Aislamiento"

    Recordemos:
    los contenedores no pueden acceder a los archivos del sistema anfitrión,
    a menos que se les de acceso explícito mediante el uso de volumenes.


### `WORKDIR`

Esta cláusula cambia el directorio de ejecución de la *shell* interna
de la imagen.
Afecta al uso del comando o intérprete interno del futuro contenedor.

!!! tip "Directorio para ejecutables"

    Una ruta habitual para ubicar los ejecutables dentro de la imagen es `/home/app`. 
    Este directorio **debe ser creado** para su uso.


### `EXPOSE`

En el Dockerfile, `EXPOSE` proporciona la metadata del puerto usado para la imagen. 
No es obligatorio incluirlo; sin embargo
es buena práctica incluirlo cuando la imagen usa puertos estáticos.

!!! info "Puertos IP"

    Los gestores de contenedores usan el protocolo IP 
    (número de IP + puerto) 
    tanto para hacer funcionar los contendores 
    como para interconectarlos.
    Por este motivo cada imagen define un número de puerto
    que será heredado por el contenedor
    para poder ser utilizado. 


En el caso de usar puertos dinámicos el `EXPOSE` es difícil de especificar.




### `CMD` y `ENTRYPOINT`


La cláusula `CMD` es típicamente la cláusula final del Dockerfile.
Es la encargada de ordenar el arranque del intérprete o comando interno
y asignarle la rutina embebida por el desarrollador.

```dockerfile title="Rutina única - CMD"
# Servidor Python 
CMD ["python", "/usr/src/myapp/server.py"]
```

de esta la rutina interna entrará en funcionamiento en cuanto arranque el contenedor. 
Por ejemplo, si la rutina implementa un servidor en Python el arranque de su contenedor es:

```bash  title="CMD - Uso en contenedor"
docker start servidor_python
```

Su alternativa es la cláusula `ENTRYPOINT` el cual permite llamar al comando o intérprete interno pero sin indicarle rutinas ni argumentos:


```dockerfile  title="Rutina externa - ENTRYPOINT"
# Intérprete Python encapsulado 
ENTRYPOINT ["python"]
```

los cuales serán pasados como argumentos del contenedor al ponerlo en marcha. Ejemplo:

```bash  title="ENTRYPOINT - Uso en contenedor"
docker start contenedor_python  rutina_externa.py
```


El uso de una de estas cláusulas es obligatoria.


## Comandos (resumen)

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