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
#   - MongoDB
---


# Redes 
<!-- 
Es posible interconectar los contenedores mediante redes puente (*bridge*) creadas en Docker. -->

<!-- Los contenedores se interconectan mediante redes puente (*bridge)*. -->


Cuando hay que conectar varios contenedores interdependientes conviene usar las redes (*networks*). 
Estas son alternativas al *port mapping*
que no necesitan configuración de puertos para funcionar.


## Administrar redes

### Listar redes

Enumera las redes ya existentes:

=== "Docker"

    ```bash title="Redes - listar"
    docker network ls
    ```
=== "Podman" 

    ```bash title="Redes - listar"
    podman network ls
    ```

### Crear red

Crea una nueva red del tipo *bridge* (puente) con el nombre especificado:

=== "Docker"

    ```bash title="Redes - crear"
    docker network create nombre_red
    ```

=== "Podman" 

    ```bash title="Redes - crear"
    podman network create nombre_red
    ```

### Eliminar red

Elimina la red indicada:

=== "Docker"

    ```bash title="Redes - eliminar"
    docker network rm nombre_red
    ```

=== "Podman" 

    ```bash title="Redes - eliminar"
    podman network rm nombre_red
    ```

## Contenedores conectados
<!-- 
Crea un contenedor que incluya conexión a la red *bridge* indicada. 
La red puente se indica como una opción más:
 -->


Al crear un contenedor nuevo se pueden incluir las opciones:

- `--network` :   especifica el nombre de red preexistente a conectar;
- `--network-alias`: funciona como un apuntador de la red al contenedor destino de la red (opcional).


### Sintaxis

La red puente se agrega como una opción más:

=== "Docker"

    ```bash title="Contenedores - conectados a red"
    docker create   --network nombre_red  --name nombre_contenedor  nombre_imagen
    ```

=== "Podman" 

    ```bash title="Contenedores - conectados a red"
    podman create   --network nombre_red   --name nombre_contenedor  nombre_imagen
    ```

!!! warning "Redes preexistentes"

    Para que la comunicación entre contenedores funcione es necesario que las redes que estos utilizan hayan sido creadas previamente.


### Inspección

Las redes se inspeccionan con el comando `dig`:

```bash title="Inspección de redes"
dig nombre-red
dig  alias-red
```

El resultado es un informe de las direcciones IP, puertos,tipo de socket, etc. que usan los contenedores.


!!! info  "DNS en networks"

    Las networks que usan los gestores de contenedores
    hacen uso de su propio servicio DNS (*Name Domain Server*) interno 
    para completar las conexiones entre los contenedores.
    Por eso no se especifican IPs ni puertos al incluirlas.



## Referencias


[Repositorio ofical de Podman - Networking básico](https://github.com/containers/podman/blob/main/docs/tutorials/basic_networking.md)


[Docker Docs - Bridge network driver](https://docs.docker.com/engine/network/drivers/bridge/)