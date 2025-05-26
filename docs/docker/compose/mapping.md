
# Port Mapping



Las imágenes poseen un puerto predefinido,






```yaml title="Port Mapping - Par único"
services:
  puerto_banner:
    image: nombre_imagen
    ports: "puerto_host:puerto_imagen"
``` 

Una misma imagen puede exponer múltiples puertos de acceso,
por este motivo el campo `ports`
admite definir una lista de paresde puertos *host-imagen*:

```yaml title="Port Mapping - Lista"
services:
  puerto_banner:
    image: nombre_imagen
    ports: 
     - "puerto_host_1:puerto_imagen_1"
     - "puerto_host_2:puerto_imagen_2"
     - "puerto_host_3:puerto_imagen_3"
``` 


## Ejemplo: banner de Podman 

La imagen `quay.io/libpod/banner` responde con un banner
al ser peticionado poer el puerto 80.

En este ejemplo se elige mapear al puerto `8081` del sistema anfitrión:

```yaml
services:
  puerto_banner:
    image: quay.io/libpod/banner
    ports: "8081:80"
``` 

la petición se hace desde la *shell* del sistema anfitrión con el comando `curl` (*Client Url*) a la direccioón local (*localhost*) y al puerto indicado (`8081`):

```bash title="Request al contenedor"
curl localhost:8081
```

La respuesta será el logo de Podman:

```
   ___          __              
  / _ \___  ___/ /_ _  ___ ____ 
 / ___/ _ \/ _  /  ' \/ _ `/ _ \
/_/   \___/\_,_/_/_/_/\_,_/_//_/

```
