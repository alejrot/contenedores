

# Networks


## Introducción

Las redes o *networks* son objetos auxiliares del gestor de contenedores
que imitan a las redes privadas de computadoras,
permitiendo que los contenedores se comuniquen entre sí
mediante el uso del protocolo IP y sus derivados.

El gestor se encarga de configurar automáticamente las redes existentes
y de asignar automáticamente una dirección IP a cada contenedor de manera que la interconexión sea posible.

!!! info "Nombre de servicio como dominio"

    El nombre de servicio asignado a cada contenedor funciona
    como un alias (un "nombre de dominio") de una dirección IP privada.
    El nombre de servicio permite a los otros contenedores establecer comunicaciones mediante protocolos basados en el protocolo IP.

    El servidor DNS interno del gestor de contenedores es el encargado de traducir los nombres de servicio asignados a cada contenedor a direcciones IP.


## Network por default

Se asume de manera predeterminada
que todos los contenedores del proyecto están interconectados
y por eso el gestor de contenedores
los asigna a una misma red salvo indicación contraria.
Es por este motivo que en muchos archivos Compose
no se crea ni configura *network* alguna.






## Ejemplo: petición con curl


En este ejemplo se crea un contenedor servidor que responde con el logo de Podman
y un contenedor cliente
con una imagen Linux que posea el comando `curl` (Client URL).




``` yaml
name: demo_network_default

services:

  cliente:
    image: registry.fedoraproject.org/fedora
    container_name: cliente_curl
    command: curl server_banner:80 
    depends_on: server_banner

  server_banner:
    container_name: server_banner
    image: quay.io/libpod/banner
```


El contenedor cliente debe ser puesto en marcha cuando el contenedor servidor ya esté disponible, por eso se incluye el parámetro `depends_on`.

La respuesta del servidor se podrá ver al desplegar el proyecto:

```bash
podman-compose up 
```

o también al consultar el log del contenedor cliente mediante el comando `logs`:

``` bash
podman logs cliente_curl 
``` 

Respuesta típica:

```
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
   ___          __              
  / _ \___  ___/ /_ _  ___ ____ 
 / ___/ _ \/ _  /  ' \/ _ `/ _ \
/_/   \___/\_,_/_/_/_/\_,_/_//_/

100   133  100   133    0     0  27131      0 --:--:-- --:--:-- --:--:-- 3325
```