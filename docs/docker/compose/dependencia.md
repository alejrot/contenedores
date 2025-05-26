# Dependencia entre contenedores

A veces los contenedores dependen
de la disponibilidad de otros para poder funcionar correctamente.
La secuencialidad de la puesta en marcha
se consigue mediante el campo `depends_on`.


## Sintaxis básica

Las dependencias se pueden definir como lista:

```yaml  title="Dependencias - Lista simple"
services:

  segundo:
    image: imagen_2
      depends_on: 
        - primero

  primero:
    image: imagen_1
```

Durante el despliegue,
el segundo contenedor se crea después del primero.
Durante la eliminación,
el segundo contenedor es removido primero.


!!! warning "`link`"

	La opción `link`, usada para comunicar un servicio/contenedor con otro, especifica también un orden de dependencias entre servicios tal como hace `depends_on`.
    Esta opción está obsoleta
    y el Compose de Podman ya no la ejecuta arrojando un error.

	```yaml
	links:
		- contenedor_2 		#contenedor destino
	```


	Más información:
	[Stack Overflow - Difference between links and depends_on in docker_compose.yml](https://stackoverflow.com/questions/35832095/difference-between-links-and-depends-on-in-docker-compose-yml)



## Opciones agregadas

Si se requieren opciones de configuración extra,
las dependencias se definen como clave:

```yaml title="Dependencias - con parámetros" 
services:

  contenedor_1:
    image: imagen_1
      depends_on: 
        contenedor_2:
            restart: true
            condition: service_started
            required: false     # true por default


  contenedor_2:
    image: imagen_2
```

El parámetro `condition` tiene varias opciones:

- `service_started`:
el servicio debe haber arrancado;
- `service_healthy`:
el servicio debe estar funcionando correctamente, 
según test definido por el parámetro `healthcheck`;
- `service_completed_successfully`: 
el servicio debe completarse exitosamente.


El parámetro `restart` ordena el reinicio del contenedor
en cuanto sus servicio requeridos estén listos.

Pr último, el parámetro `required` especifica si es obligatorio que el servicio apuntado haya sido arrancado o esté disponible.
Si es seteado como `false`
entonces el comando `compose` sólo advertirá
en caso que el servicio requerido no está disponible o no inició.



## Ejemplo de uso

Este demo sencillo crea dos contenedores, los cuales muestran la hora actual y se cierran automáticamente.

```yaml 
name: demo_depends_on

services:

  fin:
    image: registry.fedoraproject.org/fedora
    command: date +"%X"
    depends_on: 
      inicio:
        condition: service_completed_successfully

  inicio:
    image: registry.fedoraproject.org/fedora
    command: date +"%X"
```

El container `inicio` siempre iniciará primero:

```
[inicio] | 03:31:08
[fin]    | 03:31:09
```