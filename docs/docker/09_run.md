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


# Comando `run`

Este comando combina varias acciones:

- Busca la imagen y si no está disponible localmente la descarga automáticamente (`pull`);
- Crea un contenedor que incluye la imagen elegida (`create`);
- Inicia el contenedor creado (`start`).

<!-- 
Esta instrucción no devuelve el control al usuario a menos que termine o se cancele. 
Si se repite el comando varias veces se crean varios contenedores parecidos, uno por comando. 
-->

## Uso

Uso básico:

=== "Docker"

    ```bash title="comando run"
    docker run nombre_imagen
    ```

=== "Podman" 

    ```bash title="comando run"
    podman run nombre_imagen
    ```

Esta instrucción no devuelve el control al usuario a menos que termine 
ó se presione ++ctrl+c++ (cancelación). 
Si se repite el comando varias veces se crean varios contenedores parecidos, uno por comando.


## Segundo plano 

La opción `-d` (*detached*) devuelve el control al usuario de inmediato:

=== "Docker"

    ```bash title="comando run - segundo plano"
    docker run  -d nombre_imagen
    ```

=== "Podman" 

    ```bash title="comando run - segundo plano"
    podman run -d nombre_imagen
    ```

de esta forma el contenedor seguirá funcionando en segundo plano.

Lo mismo pero añadiendo el *port mapping*:

=== "Docker"

    ```bash title="comando run - port mapping"
    docker run --name nombre_contenedor -p puerto_anfitrion:puerto_imagen -d nombre_imagen
    ```

=== "Podman" 

    ```bash title="comando run - port mapping"
    podman run --name nombre_contenedor -p puerto_anfitrion:puerto_imagen -d nombre_imagen
    ```


## Contenedores descartables

La opción `--rm` permite crear contenedores descartables. 
Éstos son eliminados automáticamente cuando se detienen. 

=== "Docker"

    ```bash title="comando run - contenedores descartables"
    docker run --rm nombre_imagen
    ```


=== "Podman" 

    ```bash title="comand run - contenedores descartables"
    podman run --rm nombre_imagen
    ```


!!! example "Ejemplo de uso: Banner de Podman"


    === "Docker"

        ```bash title="Contenedor de Banner"
        docker run -dt --name PodmanBanner --replace  -p 8081:80 quay.io/libpod/banner
        ```

    === "Podman" 
    
        ```bash title="Contenedor de Banner"
        podman run -dt --name PodmanBanner --replace  -p 8081:80 quay.io/libpod/banner
        ```

    ```bash title="Request al contenedor"
    curl localhost:8081
    ```

    Logo respuesta:
    
    ```
       ___          __                 
      / _ \___  ___/ /_ _  ___ ____    
     / ___/ _ \/ _  /  ' \/ _ `/ _ \    
    /_/   \___/\_,_/_/_/_/\_,_/_//_/   
        
    ```


