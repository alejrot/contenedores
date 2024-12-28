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

# Pods

Las vainas o (*pods*) son una especie de contenedores auxiliares que sirven para englobar múltiples contenedores internos,
creando unidades funcionales completas.

Podman permite la creación manual de pods
en tanto que Docker no.

Por otra parte,
Kubernetes crea y usa *pods* de forma automática: un *pod* por cada "nodo" (servidor físico o lógico) dedicado al procesamiento del software desplegado.



## Crear pods


```bash 
podman pod create --name nombre_pod
```


```bash 
podman pod create --label nombre_pod
```


## Listar pods



```bash 
podman pod list
podman pod ps 
```



## Agregar contenedores

```bash 
podman pod run -dt  --pod  nombre_pod   imagen:version  nombre_contenedor
```




```bash 
podman pod top nombre_pod
```

```bash 
podman pod stats -a --no-stream
```

```bash 
podman pod inspect nombre_pod
```


## Detener contenedores

```bash 
podman pod stop nombre_pod
```


## Listar pods y contenedores

```bash 
podman ps -a --pod
```



## Eliminar pods


```bash 
podman pod rm nombre_pod
```










!!! example "Pod de sitios MkDocs"

    ```bash 
    podman pod create --label PaginasMkDocs
    podman pod run -dt  --pod  PaginasMkDocs docker.io/squidfunk/mkdocs-material:latest PythonDocs
    ```