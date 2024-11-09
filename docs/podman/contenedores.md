---
tags:
#   - HTML5
  # - JavaScript
  # - CSS
#   - YAML
#   - MkDocs
#   - Python
#   - Docker
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

# Imágenes y contenedores

Las imágenes son los ejecutables compactados ,los cuales pueden incorporar intérpretes de lenguajes, bibliotecas, frameworks, etc.


Los contenedores son los objetos que "rodean" las imágenes y que incorporan las configuraciones, rutinas de programa, etc. 

## Imágenes

### Listado

Indica qué imágenes están descargadas en la PC y sus características básicas: versión, identificador, espacio ocupado, etc.

```bash title="Listar imágenes locales"
podman images
```

!!! info "DockerHub"

    En [DockerHub](https://hub.docker.com/) se puede consultar qué imágenes hay disponibles e indica el comando completo para descargarlas. 


!!! info "Fuentes de imágenes"

    - [Docker](docker.io/)
    - [Fedora Project](registry.fedoraproject.org/)
    - [Red Hat](registry.access.redhat.com/) (Es de pago)
    - [Quay](quay.io/)


### Descarga

Descarga la última versión disponible de la imagen indicada desde DockerHub y la etiqueta como latest.

```bash title="Descargar imagen - Última release"
podman pull nombre_imagen
```
Descarga la versión indicada de la imagen  y la etiqueta numerada.Si no se indica la versión se descarga la más reciente (latest).

```bash title="Descargar imagen - version específica"
podman pull nombre_imagen:numero_version
```

!!! warning "Opciones de imagen"

    Cada imagen puede tener distintas opciones de configuración. Por ello hay que chequear también las opciones que se requieren para configurar los futuros contenedores basados en estas imágenes. 


### Borrado

Elimina del disco la imagen especificada:
```bash title="Eliminar imagen local"
podman image rm nombre_imagen:numero_version
```



## Contenedores

### Creación

`podman create` crea un contenedor nuevo basado en la imagen especificada. Devuelve un número de identificador (ID).

```bash title="Crear contenedor - nombre arbitrario"
podman create nombre_imagen
```

```bash title="Crear contenedor - con nombre"
podman create --name nombre_contenedor nombre_imagen
```


<!-- Crea un contenedor nuevo basado en la imagen especificada con el nombre indicado. -->

### Arranque

La puesta en marcha de los contenedores se puede hacer en base a su número identificador o en base a su nombre.

<!-- Pone en funcionamiento el contenedor identificado por ID. -->
```bash title="Arranque - por ID"
podman start id_contenedor
```
<!-- Pone en funcionamiento el contenedor indicado por nombre. -->
```bash title="Arranque - por nombre"
podman start nombre_contenedor
```

Agregando las opciones `-a` (adjuntar) e `-i` (interactivo) se puede ver el log en consola y ejecutar comandos:

```bash title="Arranque - interactivo"
podman start -a -i nombre_contenedor
```
Las opciones se pueden indicar juntas:

```bash title="Arranque - interactivo"
podman start -ai nombre_contenedor      
```


### Detención

Detiene al contenedor indicado:
```bash title="Detención - por nombre"
podman stop nombre_contenedor
```

!!! info "Señales de parada"

    En Linux la orden `stop` envía la señal del sistema SIGTERM. Si el contenedor no se detuvo en 10 segundos (valor predeterminado) Podman le envía la señal SIGKILL, lo cual equivale a lanzar la orden `kill`:

    ```bash title="Detención - SIGKILL"
    podman kill nombre_contenedor
    ```

!!! tip "Otras señales"
    Cabe señalar que el comando kill permite mandar otras señales diferentes con ayuda de la opción `--signal`:

    ```bash title="Señales específicas"
    podman kill --signal="SIGUP" nombre_contenedor
    ```
    En el ejemplo, la señal SIGUP hace que la aplicación relea sus archivos de configuración, siempre y cuando la aplicación lo soporte.



### Listado

Indica los contenedores en funcionamiento:

```bash title="Listado - en funcionamiento"
podman ps
```
Indica todos los contenedores existentes:

```bash title="Listado - todos"
podman ps -a
```

### Borrado

Elimina el contenedor indicado
```bash title="Borrado de contenedor"
podman rm nombre_contenedor
```



### Permisos de ejecución (REVISAR)


A diferencia de Docker, Podman no ejecuta sus contenedores con permisos de administrador. Como contrapartida, requiere que el usuario actual tenga permisos para ejecutar cada contenedor. Para liberar los contenedores a éstos se los crea con la opción: `--security-opt label=disable`.

De esta forma, la creación del contenedor queda como:


```bash title="Crear contenedor - con nombre"
podman create --security-opt label=disable --name nombre_contenedor nombre_imagen
```


### Puertos y Port Mapping


Cada contenedor se comunica con otros contenedores y aplicaciones usando *sockets* , los cuales se componen por dirección IP y número de puerto. Cada imagen tiene un número de puerto asignado, el cual hereda el contenedor de manera predeterminada.  Por este motivo varios contenedores activos pueden heredar un mismo número de puerto. Para evitar ambigüedades y poder comunicarse con todos ellos los puertos de los contenedores se pueden “mapear” a distintos  puertos del sistema anfitrión.

Crea el contenedor ya mapeado al puerto host deseado. ( opción `-p`: *publish* ):

```bash title="Contenedor - con port mapping"
podman create  -p puerto_anfitrion:puerto_imagen --name nombre_contenedor nombre_imagen
```



### Variables de Entorno


Las variables de entorno se agregan al contenedor durante su creación mediante la opción `-e`: 


```bash title="Crear contenedor - con variable entorno"
podman create -e VARIABLE=VALOR  nombre_imagen
```

De usarse varias variables de entorno éstas se separan con barras invertidas `\`:


```bash title="Crear contenedor - con varias variables entorno"
podman create \ -e VARIABLE_1=VALOR_1 \ -e VARIABLE_2=VALOR_2 \ nombre_imagen
```

!!! example "Base de datos: Usuario y contraseña"

    En el ejemplo se hace un contenedor de MongoDB (base de datos no relacional) al que se le asigna nombre de usuario y contraseña:


    ```bash title="MongoDB - con usuario y contraseña" hl_lines="2 3"
    podman create  \
	-e MONGO_INITDB_ROOT_USERNAME=mongoadmin \
	-e MONGO_INITDB_ROOT_PASSWORD=secret \
	--name mongo_admin  mongo 
    ```
    




### Logs

El comando `logs` da los reportes del container usado hasta el presente y devuelve el control de la terminal al usuario.

```bash  title="mensajes de log"
podman logs nombre_contenedor
```

Agregando la opción `--follow` la terminal queda en espera a nuevos eventos:


```bash title="mensajes de log - modo live"
podman logs --follow nombre_contenedor
```

!!! hint "Atajo de salida"
    Se sale del *modo live* con ++ctrl++ + ++c++


### Inspeccionar metadatos


Los metadatos se leen con el comando `inspect`:

```bash  title="inspeccionar"
podman inspect nombre_contenedor
```

La metadata se devuelve como objeto JSON (pares clave - valor). Si se busca solamente un parámetro particular se usa la opción `--format`:

```bash  title="inspeccionar - parametro IP"
podman inspect  --format='{{.NetworkSettings.IPAddress}}' nombre_contenedor
```



### Run


Combina varios comandos:

1. Busca la imagen y la descarga si no existe;
2. Crea un contenedor que incluye la imagen elegida;
3. Inicia el contenedor creado.

Esta instrucción no devuelve el control al usuario a menos que termine o se cancele. Si se repite el comando varias veces se crean varios contenedores parecidos, uno por comando.

Uso básico:

```bash title="run"
podman run nombre_imagen
```

La opción `-d` (*dettached*) le devuelve el control al usuario de inmediato. El contenedor seguirá funcionando en segundo plano

```bash title="run - segundo plano"
podman run -d nombre_imagen
```

Lo mismo pero añadiendo el *port mapping*:

```bash title="run - port mapping"
podman run --name nombre_contenedor -p puerto_anfitrion:puerto_imagen -d nombre_imagen
```

La opción `--rm` permite crear contenedores descartables. Éstos son eliminados automáticamente cuando se detienen. 


```bash title="run - contenedores descartables"
podman run --rm nombre_imagen
```


!!! example "Ejemplo de uso: Banner de Podman"

    ```bash title="Contenedor de Banner"
    podman run -dt --name PodmanBanner --replace  -p 8081:80 quay.io/libpod/banner
    ```

    ```bash title="Request al contenedor"
    curl localhost:8081
    ```
    ``` title="Logo respuesta"
       ___          __              
      / _ \___  ___/ /_ _  ___ ____ 
     / ___/ _ \/ _  /  ' \/ _ `/ _ \
    /_/   \___/\_,_/_/_/_/\_,_/_//_/
    ```





## Redes puente

Los contenedores se interconectan mediante redes puente (bridge).

Enumera las redes creadas por los contenedores:

```bash title="Redes - listar"
podman network ls
```

Crea una nueva red del tipo *bridge* (puente) con el nombre especificado.

```bash title="Redes - crear"
podman network create nombre_red
```

Elimina la red indicada:
```bash title="Redes - eliminar"
podman network rm nombre_red
```


!!! warning "Redes preexistentes"
    
    Para que los contenedores funcionen conectados a una *red bridge* es necesario que las redes que utilizan hayan sido creadas previamente.



Crea un contenedor que incluye conexión a la red puente indicada:

```bash title="Contenedores - conectado a red"
podman create [...]  --network nombre_red  [...]
```






## Referencias


[Repositorio ofical de Podman - Networking básico](https://github.com/containers/podman/blob/main/docs/tutorials/basic_networking.md)





