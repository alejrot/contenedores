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

# Optimizar Imágenes


## Ventajas

- Despliegues y registros más veloces;
- Reducir costos de almacenamiento en nube;
- Minimzar vulnerabilidades internas;
- Arranques de contenedores más veloces;
- Manejo de contenedores más eficiente.


## Imágenes minimalistas

Un buen punto de partida consiste en elegir una imagen 
lo más reducida posible (*minimal flavors*)
en cuanto a espacio ocupado y componentes internos.


Las distribuciones de Linux más habituales son Ubuntu, Debian y Alpine.
Entre ellas, Alpine es con diferencia la más liviana y reducida:

```Dockerfile title="Imagen compacta"
FROM alpine
``` 
sin embargo esta opción no da el mejor rendimiento para todos los lenguajes posibles, 
por ello debe ser usada con precaución.


Una alternativa aún más compacta que `alpine` es el uso de imágenes *distroless*, por ejemplo:

```Dockerfile title="Imagen distroless"
# Imagen 'distroless'
FROM gcr.io/distroless/static-debian11
``` 



## Dockerignore


El archivo oculto `Dockerignore` se usa para 
prevenir la copia de contenidos innecesarios
adentro de la imagen objetivo:

- carpetas de controles de versiones como Git (`.git`) o Subversion (`.svn`);
- paquetes descargados;
- archivos `.env`  y claves SSH;
- etc.

Se usa de idéntica manera que el archivo `gitignore` de Git.
Ejemplo de uso:

```Dockerfile title="Archivo Dockerignore - Ejemplo"
# Control de versiones
.git
.svn
.gitignore

# Documentación Markdown
*.md
``` 

## Construcción Multi-Stage

Con este recurso se construyen las imágenes en dos etapas:

- La primera etapa sirve para instalar todas las dependencias
y, de ser necesario, compilar archivos ejecutables.
A esta etapa se la suele llamar `builder` (constructora).
- La segunda etapa consiste en copiar los archivos estrictamente necesarios a la imagen final, la cual está basada en una imagen compatible lo más minimalista posible.

Así se ve un Dockerfile multietapa:

```Dockerfile title="Dockerfile - Multi-Stage"
# Etapa construccion
FROM imagen_builder AS builder
RUN  comandos_instalacion
# ...

# Etapa final
FROM imagen_final
# ...
COPY  --from=builder  ruta_builder  ruta_final
CMD ["comando", "argumento"]
```

Para que la imagen final funcione correctamente 
es necesario que la imagen final de referencia sea compatible 
con la imagen usada para la construcción.
Por ejemplo, si la imagen final es una imagen de Alpine Linux
es prudente elegir una imagen *builder* derivada de Alpine Linux.

```Dockerfile title="Dockerfile - Multi-Stage con Alpine"
# Etapa construccion
FROM golang:1.21-alpine AS builder
# ...

# Etapa final
FROM alpine:3.18
COPY -FROM=builder ruta_app
# ...
```

El mismo criterio se aplica para imágenes basadas en Debian y Ubuntu.



Otras etapas posibles: `build`, `dev`, `test`, `lint`.



!!! tip "Dockerfile con test"


    Si se crea la imagen en varias etapas y se desea incorporar una etapa de tests unitarios
    estos pueden tener su propia etapa:


    ```Dockerfile title="Etapas Builder y Test"
    FROM imagen1:version AS builder
    # ... (instrucciones instalacion)

    FROM imagen2:version AS TEST
    COPY -FROM=builder ruta_app
    CMD [test]


    FROM imagen2:version AS builder
    COPY -FROM=builder ruta_app
    CMD [orden]
    ```




## Optimizar capas

Cada instrucción del Dockerfile crea una nueva capa (*layer*) de la nueva imagen.

Para minimizar las capas de la imagen final y optimizar su tamaño se pueden seguir varias estrategias complementarias.

### Ordenar instrucciones

Para una mejor construcción es mejor colocar las instrucciones que cambian con menos frecuencia al final.
De esta manera se reutilizan las capas preconstruidas en caso que no se detecten cambios.

Por ejemplo, 
las rutinas de programación cambian frecuentemente 
y por ello 
es mejor copiarlas adentro de la imagen al final, 
justo antes de ordenar la ejecución.

En cambio,
las dependencias cambian con poca frecuencia 
y por eso se las instalaciones se hacen cerca del comienzo. 



### Combinar comandos `RUN`

Las instrucciones se instalación de dependencias múltiples
se pueden combinar para dar forma a una única capa.

Por ejemplo, 
si el archivo `Dockerfile` tiene originalmente esta forma:

```Dockerfile title=""
# instalación de dependencias
RUN instalar dependencia_1
RUN instalar dependencia_2
RUN ...
RUN instalar dependencia_n
# borrado de caché
RUN rm ruta_cache
```
entonces se creará un *layer* por cada dependencia.
Los *layers* se unifican de esta manera:

```Dockerfile title=""
RUN instalar dependencia_1 \
    dependencia_2 \
    ... \
    dependencia_n 
&& rm ruta_cache
```
donde el símbolo
`&&` permite concatenar comandos 
en tanto que
`\` permite concatenar los argumentos de un mismo comando.



!!! tip "Limpiar layers"

    Es una buena práctica aprovechar 
    cada instrucción RUN relacionada con la instalación de dependencias
    para eliminar
    los archivos descargados y desempaquetados dentro de la imagen.




### Usar `COPY` en vez de `ADD`

[Docker Docs - `ADD` or `COPY`](https://docs.docker.com/build/building/best-practices/#add-or-copy)




## Comando `history`

El comando `history` sirve para listar el historial de capas y sus cambios a lo largo del tiempo.

=== "Docker"
    
    ```bash title="Historial de capas"
    docker history imagen:tag
    ```

=== "Podman"

    ```bash title="Historial de capas"
    podman history imagen:tag
    ```





## Comando `scout`


El comando `scout` sirve para buscar vulnerabilidades conocidas en la imagen objetivo y reportarlas.


```bash title="Buscar vulnerabilidades"
docker scout cve imagen:tag
```

(Sólo disponible en Docker).




## Referencias

[DevDojo.com - Optimizing Docker Image Sizes: Advanced Techniques and Tools](https://devdojo.com/bobbyiliev/optimizing-docker-image-sizes-advanced-techniques-and-tools)


