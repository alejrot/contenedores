
# Docker

[Aprende Docker ahora! curso completo gratis desde cero!](https://www.youtube.com/watch?v=4Dko5W96WHg)

[Manuales oficiales de Docker](https://docs.docker.com/reference/)



Docker es un administrador de **contenedores** de software. Los contenedores facilitan el desarrollar, compartir y poner en marcha el nuevo software minimizando los problemas de dependencias, actualizaciones etc. Los contenedores son una versión simplificada de las máquinas virtuales, permitiendo la ejecución de un kernel (sistema operativo) y software adicional (frameworks, bibliotecas, etc ) dentro de un sistema con otro sistema operativo. 

## Despliegue sin contenedores

### Requiere:
- código
- lista de dependencias 
- archivos de configuración
### Problemas
- Distintas versiones de dependencias
- Problemas de comunicación
- Los problemas se detectan al desplegar → requerirá una vuelta atrás (rollback)

## Despliegue con contenedores

El equipo de desarrollo y el de operaciones crean en conjunto una imagen. La imagen depende del runtime de Docker. El proceso puede automatizarse con los pipelines  de los servicios de controladores de versión (github, bitbucket,etc)

### Imagen:
Es un empaquetado con las dependencias, el código ,variables de entorno, etc
Cada imagen tiene un software distinto: sistema operativo invitado, biblioteca, framework, etc. Distintas versiones del software elegido implican distintas imágenes. Sin embargo, las imágenes pueden compartir componentes comunes para ahorrar espacio en disco.

### Contenedor
Es una apilado de imágenes capaz de ser ejecutado.
La capa inferior del contenedor es el kernel elegido. Es muy popular usar Alpine Linux por su pequeño tamaño.


## Docker vs Virtual Machines (VM)

El problema puede pensarse en tres capas:
- Aplicaciones
- Kernel
- Hardware


## Modos virtualización:
- Para virtualización: el kernel anfitrión (host) da soporte al kernel invitado (guest) de cada máquina virtual. Esta virtualización intenta dar tanto  acceso a los kernels invitados al hardware como sea posible.
- Virtualizacion Parcial: algunos componentes del hardware se virtualizan para el SO guest, haciendo que éste no pueda acceder a ellos directamente.
- Virtualizacion Completa: todos los recursos de hardware son virtualizados, ergo los kernels invitados no tienen acceso directo.

Docker usa el kernel anfitrión directamente:
- Las imágenes pesan mucho menos que las máquinas virtuales
- Los contenedores arrancan casi de inmediato
- El rendimiento es mucho mejor

Docker Desktop
- Es una VM que corre Linux y ejecuta containers 
- permite acceder al sistema de archivos y a la red , tanto interna como externa
- Incluye herramientas como Docker Compose, Command Line Interface (CLI), etc
- Corre nativo en Windows gracias a WSL2 (Windows Subsystem for Linux)


## Comandos

Estos comandos se ejecutan en la terminal (recomendado: Bash). Hay que asegurarse primero de que Docker Desktop está no sólo inicializado sino también corriendo.

<!-- ## Imágenes


Indica qué imágenes están descargadas en la PC y sus características básicas: versión, identificador, espacio ocupado, etc.

    docker images

Descarga la última versión disponible de la imagen indicada desde docker hub y la etiqueta como latest.

    docker pull <nombre-imagen>

Descarga la versión indicada de la imagen  y la etiqueta numerada.Si no se indica la versión se descarga la más reciente (latest).

    docker pull <nombre-imagen>:<número-version>

En docker hub se puede consultar qué imágenes hay disponibles e indica el comando completo para descargarlas. 


**Importante:** hay que chequear también los comandos para configurar los futuros contenedores basados en estas imágenes.


Elimina del disco la imagen especificada:

    docker image rm <nombre-imagen>:<número-version> -->


<!-- ## Contenedores


    docker create <imagen_base>
    docker container create <imagen_base>

Crea un contenedor nuevo basado en la imagen especificada. Devuelve un número de identificador (ID) 

    docker create --name <nombre_contenedor> <imagen_base>

Crea un contenedor nuevo basado en la imagen especificada con el nombre indicado.

    docker start <ID>

Pone en funcionamiento el contenedor identificado por ID.

    docker start <nombre_contenedor>

Pone en funcionamiento el contenedor indicado por nombre.

    docker stop <nombre_contenedor>

Detiene al contenedor indicado

    docker ps

Indica los contenedores en funcionamiento.

    docker ps -a

Indica todos los contenedores existentes

    docker rm <nombre_contenedor>

Elimina el contenedor indicado -->

<!-- ## Puertos y Port Mapping

Cada contenedor tiene un número de puerto asignado, eso le permite la comunicación con otros contenedores y aplicaciones usando sockets. Dos contenedores activos pueden tener un mismo número de puerto. Para evitar ambigüedades y poder comunicarse con ambos los puertos de los contenedores se pueden “mapear” a distintos  puertos del sistema anfitrión


    docker create  -p <puerto_anfitrion>:<puerto_contenedor> --name <nombre_contenedor> <imagen_base>

Crea el contenedor ya mapeado al puerto host deseado. (  -p: publish ). Importante: no poner espacios entre números de puerto y signo ‘:’ .

    docker logs <nombre_contenedor>

Da los reportes del container usado y devuelve el control de la terminal al usuario.

    docker logs --follow <nombre_contenedor>

Da los reportes del container elegido y espera a nuevos sucesos del mismo. 
**HINT:** se sale del modo live con ‘ctrl’+’C’.

    docker run <imagen_base>

Combina varios comandos:
1. Busca la imagen y la descarga si no existe;
2. Crea un contenedor que incluye la imagen elegida;
3. Inicia el contenedor creado.
Esta instrucción no devuelve el control al usuario a menos que termine ó se presione ‘ctrl’+’C’ (cancelación). Si se repite el comando varias veces se crean varios contenedores parecidos, uno por comando.

    docker run  -d <imagen_base>

Lo mismo que antes, pero la opción -d (dettached) devuelve el control al usuario de inmediato.

    docker run --name <nombre_contenedor> -p <puerto_anfitrion>:<puerto_contenedor> -d <imagen_base>

Lo mismo pero añadiendo el port mapping. -->


<!-- ## Variables de Entorno

    docker create  […]  -e [...]

Crea el container con las variables de entorno ya configuradas (opcion -e) -->



<!-- ## Redes en Docker

Es posible interconectar los contenedores mediante redes puente (bridge) creadas en Docker.

    docker network ls

Enumera las redes creadas por los contenedores

    docker network create <nombre_red>

Crea una nueva red del tipo bridge (puente) con el nombre especificado. 

    docker network rm <nombre_red>

Elimina la red indicada

    docker create [...]  --network <nombre_red>  [..]

Crea un contenedor que incluye conexión a la red bridge indicada.

Para que los contenedores funcionen es necesario que las redes que utilizan hayan sido creadas previamente. -->


## Archivo Dockerfile

El archivo Dockerfile nos permite crear nuevas imágenes en base a una preexistente donde se incluyan nuevos comandos y aplicaciones. A este archivo no se le puede cambiar el nombre.


### Sintaxis

La sintaxis es la siguiente:

```dockerfile
FROM <imagen_base>:<versión>		# (qué imagen de Docker será afectada)

RUN mkdir -p <ruta_destino>		# (se crea el directorio de destino)(tipicamente /home/app si es contenedor con Linux)

RUN <comandos_instalacion_aplicacion>  # (opcional)

COPY . <ruta_destino>		# (completar con la ruta que corresponda)

EXPOSE <numero_puerto>			# (indica el puerto del contenedor)

CMD [ <comando> , <ruta_destino/ejecutable> ]
```
El comando RUN permite instalar aplicaciones dentro de la imagen de ser necesario. Esto se hace indicando los  
Es posible indicar un directorio de funcionamiento con el comando WORKDIR, de este modo la sintaxis quedaría así:

```dockerfile
[...]


WORKDIR <ruta_destino>

[...]

CMD [<comando> , <ejecutable>]
```



### Nuevas imágenes modificadas

    docker build -t <nombre_aplicacion>:<numero_version>  <ruta_a_archivos>

Construye una imagen a partir de un archivo Dockerfile disponible en la ruta indicada. Si la terminal ya está ubicada en el directorio de la aplicación poner un punto. 
Ejemplo: ‘docker build -t miapp:1 . ‘

    docker build -t <nombre_aplicacion>:<numero_version>  .

Lo mismo pero en directorio actual. El archivo Dockerfile debe estar aquí.



## Docker Compose


Habilita una plantilla para facilitar  la creación de los contenedores. El archivo de configuración se llama `docker-compose.yml`   (archivo YAML). Dentro del archivo debe respetarse estrictamente la indentación sino no funciona.También hay que respetar las comillas para los números de puerto y los guiones donde se indican. Docker Compose aprovecha el archivo Dockerfile por defecto para completar la configuración del proyecto.
Los comentarios son precedidos del carácter ’#’

Sintaxis (ejemplo):
```yml
version:  “<numero_version>”
services:
	<contenedor_1>:
		build:  .  			#(mismo directorio)
		ports:
“<puerto_host_1>:<puerto_contenedor_1>”
 “<puerto_host_2>:<puerto_contenedor_2>” #respetar las comillas dobles para los puertos
 [...]		#(pueden mapearse tantos puertos como se necesite )
		links:
 <contenedor_2> 		#(contenedor destino)
	<contenedor_2>:
		image: <imagen_2>
		ports: 
”<puerto_host_n>:<puerto_contenedor_n>”
		environment:
<VARIABLE_ENTORNO_1>=<valor_1>
<VARIABLE_ENTORNO_2>=<valor_2>
		volumes:
<contenedor_2-data>:  <ruta_data_2>

volumes:
	<contenedor_2-data>:	# Los volumes son opcionales 
					#se explican más adelante

```

### Creación Automática

    docker compose up

Crea los contenedores deseados y las imágenes necesarias en base al archivo docker-compose.yml. También crea las redes necesarias por defecto.
Una vez terminado los hace funcionar. Puede interrumpirse haciendo ‘Ctrl’ +’C’.

    docker compose down

Elimina los contenedores diseñados con el archivo `docker-compose.yml`. También elimina las redes comunes entre contenedores.



### Volumes

Definir los volúmenes ( volumes ) previene la eliminación de datos internos de los contenedores cuando se eliminan estos últimos. Los datos se mantienen fuera del contenedor, en el directorio del sistema anfitrión que se montó (es decir el volumen elegido). Estos datos pueden ser registros del funcionamiento del contenedor, bases de datos creadas en la ejecución, rutinas de lenguajes interpretados por el contenedor que se requiere poder actualizar desde fuera,etc.

Tipos volúmenes:
- anonimos: solo indica la ruta de origen a montar. Docker guarda donde le parece. No es referenciable desde afuera.
- De anfitrión o host:se indica qué carpeta montar y adonde
- Nombrado:  similar al anónimo puede ser referenciado por múltiples contenedores



## Rama desarrollo

Es posible crear dos archivos alternos de configuración para crear una rama de desarrollo (development) de los contenedores sin afectar la rama que yá está operando. Para esto se crean dos archivos adicionales llamados Dockerfile.dev y docker-compose-dev.yml
Este segundo archivo debe apuntar al primero, esto se hace añadiendo las siguientes lineas:
```
#(dentro del archivo docker-compose-dev.yml)
version:  “<numero_version>”
services:
	<contenedor_1>:
		context:  .  			#(mismo directorio del archivo .dev)
		dockerfile: Dockerfile.dev  #se carga la plantilla de desarrollo
		ports:
```

Para operar con estos archivos se usa la opción -f del comando compose:

```
docker compose -f docker-compose-dev.yml up
```



## Hot Reload

????? 

Configuración de Docker: WSL

El WSL (Windows Subsystem for Linux) es el soporte para los containers de Docker.Hay dos versiones de WSL disponibles ,la 1 que viene activada por defecto y la 2 que tiene algunas mejoras funcionales y mejor rendimiento. A cada una se le puede asignar una imagen de kernel, sea de una distribución Linux (las más habituales son Alpine, Debian y Ubuntu) o incluso Windows. La distribución más popular es Alpine Linux por sus bajos requisitos de memoria y de espacio en disco, además de su gran velocidad de ejecución.Kernel Alpine descargable desde github

¿Qué distros tengo en WSL? (Win 10)

    wsl --list

Enumera qué distribuciones están instaladas y cuál lo está por defecto. Si no hay ninguna actúa una imagen predeterminada de Docker llamada docker-desktop, la cual tiene un importante consumo de recursos.

    wsl -d 		<nombre_distro>

Arranca la distribución elegida.

    wsl --set-default  	<nombre_distro>
    wsl -s 			<nombre_distro>

Elegimos qué distribución es la que queremos usar por defecto.

    wsl --list --verbose

Indica qué versión de WSL hace funcionar a cada kernel instalado y si está corriendo o no.

    wsl --set-version <nombre_distro> <numero_version>

Elige qué versión de WSL ejecuta el kernel elegido.
HINT: ejecutar este comando con la terminal ubicada en la direccion del kernel añadido, esto es para prevenir errores tipo “el archivo del kernel no debe estar comprimido” etc.

    wsl --set-default-version <numero_version>

Eligé qué version de WSL ejecutar habitualmente (se recomienda la 2)

    wsl --shutdown

Reinicio del subsistema de linux

Para eliminar una distribución de linux del WSL se puede hacer:
    
    wsl --unregister  <distribucion>

https://www.windowscentral.com/how-completely-remove-linux-distro-wsl

    wsl --import <distribucion> <direccion_ instalacion>


Configuracion en Windows: wsl.conf y .wslconfig
wsl.conf → Archivo en la distribución Linux / Docker usada. Ubicacion: /etc/wsl.conf
.wslconfig → Archivo para Windows que va en la carpeta de usuario (si no existe se crea)

## Compartir imagen en Docker Hub

    docker login

Nos identificamos para que el servidor nos permita subir la imagen a nuestro nombre

    docker tag <grupo_imagenes> <mi_imagen>:<version>

Se indica el nombre de nuestra imagen personalizada, su número de versión y se etiqueta con el grupo de imágenes al que pertenece (ej: si nuestra imagen es una actualización de NodeJS la etiquetamos como NodeJS de modo que aparezca publicada con las demás versiones).

    docker push <mi_imagen>:<version>

Ordenamos el envío de nuestra imagen al servidor para publicarla.


## Ejecutar aplicaciones internas

[DOCKER De NOVATO a PRO! (CURSO COMPLETO EN ESPAÑOL)](https://youtu.be/CV_Uf3Dq-EU?list=PLEI4UgZ2UclHp7pugJExZhRUGBuNhMGsN&t=1166)

    docker exec <nombre_contenedor> <nombre comando>

Ejecuta un comando o aplicación interno del contenedor

    docker exec -it <nombre_contenedor> <nombre comando>

Ejecuta un comando o aplicación del contenedor de forma interactiva
Ejemplo: abrir una shell del contenedor y explorar archivos internos
    
    docker exec -it <contenedor> sh  
    # ls





[Alternativas a Docker ](https://www.cloudzero.com/blog/docker-alternatives)

[CRI - Alternativa a Kubernetes](https://cri-o.io)

[RUNC , OCI y CRI](https://www.tutorialworks.com/difference-docker-containerd-runc-crio-oci/)

[Open Container Initiative](https://opencontainers.org/about/overview/)


[FlatPack vs Snap](https://opencontainers.org/about/overview/)

[Podman (Docker Sin Daemon)](https://podman.io/whatis.html)

[Las ventajas de Podman sobre Docker](https://pandorafms.com/blog/what-is-podman/)

[Podman Desktop](https://podman-desktop.io/downloads/Linux)
[Usando el Docker Compose en Podman](https://www.redhat.com/sysadmin/podman-docker-compose)


[Guia de Contenedores - Red Hat](https://access.redhat.com/documentation/es-es/red_hat_enterprise_linux/8/html/building_running_and_managing_containers/index)
