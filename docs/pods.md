# Pods


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