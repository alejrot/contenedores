

# Compose

## Introducción

El comando Compose habilita usar una plantilla para facilitar  la creación de los contenedores y su interconexión. 

El comando `docker compose` viene integrado por defecto en Docker;
en cambio Podman no lo trae incorporado sino que debe ser instalado y habilitado como extensión 
y se puede invocar tanto por `docker-compose` como por `podman compose` 

El comando presupone la existencia de un archivo de configuración, 
tal como se explica a continuación.

## Archivos Compose

El archivo de configuración típico se llama `docker-compose.yml`.  

Dentro del archivo debe respetarse estrictamente la indentación sino no funciona.
También hay que respetar las comillas para los números de puerto y los guiones donde se indican. 
Los comentarios son precedidos del carácter `#`.
En caso de indicarse,
Docker Compose aprovecha el archivo Dockerfile por defecto para completar la configuración del proyecto.




## Contenidos



{{ pagetree(siblings) }}









## Referencias

[Docker Hub - Compose file reference](https://docs.docker.com/reference/compose-file/)



[GitHub - The Compose Specification](https://github.com/compose-spec/compose-spec/blob/main/spec.md#long-syntax-1)