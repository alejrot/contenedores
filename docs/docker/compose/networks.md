

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


## Redes custom

Los servicios pueden ser interconectados
por redes definidas por el usuario.
Estas redes requieren:

- ser agregadas al parámetro `networks` de cada contenedor que la requiera;
- ser definidas bajo la sección `networks` del archivo Compose.

Por ejemplo, 
en un sistema donde hay un contenedor que implementa un servidor
y un contenedor que implementa un cliente
ambos deben compartir una misma red:


``` yaml title="Redes - Creación y uso"
services:

  cliente:
    image: imagen_1
    # conexion a red
    networks:
      - red_usuario

  servidor:
    image: imagen_2
    # conexion a red
    networks:
      - red_usuario

# creación de red - configuracion por default
networks:
  red_usuario:
```


## Network por default

Se asume de manera predeterminada
que todos los contenedores del proyecto están interconectados
y por eso el gestor de contenedores
los asigna a una misma red salvo indicación contraria.
Es por este motivo que en muchos archivos Compose
no se crea ni configura *network* alguna.


De esta manera,
una configuración de red tácita como esta:

``` yaml
services:

  cliente:
    image: imagen_1

  servidor:
    image: imagen_2

```

es equivalente al uso de la red `default`
tal como se muestra:

``` yaml
services:

  cliente:
    image: imagen_1
    networks:
      default: {}

  servidor:
    image: imagen_2
    networks:
      default: {}
```

Al realizar las conexiones con el parámetro `network`
de cada contenedor, su conexión a la red `default` se anula automáticamente. 


## Servicios desconectados

La conexión de los contenedores a la red `default` puede impedirse 
cambiando el parámetro `network_mode` a `none`:

``` yaml
services:

  aislado_1:
    image: imagen_1
    network_mode: none  # conexiones anuladas

  aislado_2:
    image: imagen_2
    network_mode: none  # conexiones anuladas
```



## Múltiples redes

Un mismo servicio puede estar conectado a múltiples redes;
no obstante es indispensable que los servicios
que requieren interconexión
compartan una misma red.
Esta propiedad es útil para limitar el acceso de unos servicios a otros.


Por ejemplo: en un sistema que implementa:

- una interfaz gráfica (*frontend*);
- una base de datos (*database*);
- un procesamiento intermedio (*backend*)


es prudente que el *frontend* no tenga conexión directa con la base de datos, por cuestiones de seguridad.
Esto se consigue definiendo dos redes separadas y que sólo tengan acceso a ellas los contenedores que las necesitan:

``` yaml
services:

  frontend:
    image: imagen_1
    networks:
      - red_frontend

  backend:
    image: imagen_2
    networks:
      - red_frontend
      - red_db
  
  base_datos:
    image: imagen_db
    networks:
      - red_db


# creación de redes - configuracion por default
networks:
  red_frontend: 
  red_backend: 
```

nuevamente, la conexión de estos contenedores a la red `default` es deshabilitado automáticamente.



## Ejemplo aplicado: petición con curl


En este ejemplo se crea un contenedor servidor que responde con el logo de Podman
y un contenedor cliente
con una imagen Linux que posea el comando `curl` (Client URL):

=== "Red usuario"

    ``` yaml title="Curl - red default"
    name: demo_network_custom

    services:

      cliente:
        image: registry.fedoraproject.org/fedora
        container_name: cliente_curl
        command: curl server_banner:80 
        depends_on: server_banner
        networks:
          - red_usuario

      server_banner:
        container_name: server_banner
        image: quay.io/libpod/banner
        networks:
          - red_usuario


    networks:
      red_usuario:
    ```

=== "Red default"

    ``` yaml title="Curl - red default"
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

Nótese que el nombre de servicio `server_banner`
es usado como nombre de dominio por parte del comando `curl`.


La respuesta del servidor se podrá ver al desplegar el proyecto:

```bash title="Componer"
podman-compose up 
```

o también al consultar el log del contenedor cliente mediante el comando `logs`:

``` bash title="Ver respuesta"
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