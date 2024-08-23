# Pods


## Crear pods




```bash 
podman pod create --label nombre_pod
```

## Agregar contenedores

```bash 
podman pod run -dt  --pod  nombre_pod   imagen:version  nombre_contenedor
```















!!! example "Pod de sitios MkDocs"

    ```bash 
    podman pod create --label PaginasMkDocs
    podman pod run -dt  --pod  PaginasMkDocs docker.io/squidfunk/mkdocs-material:latest PythonDocs
    ```