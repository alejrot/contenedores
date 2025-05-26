


https://podman.io/docs/installation


## Instalar

Fedora (DNF)

``` bash
sudo dnf -y install podman

sudo dnf -y install podman-machine

# opcional
sudo dnf -y install slirp4netns



sudo dnf install podman-compose.noarch
```

## Crear máquina local

``` bash
podman machine init
```

## Arrancar

``` bash
podman machine start
```


``` bash
podman info
```



## CONSTRUIR TODO:

``` bash
podman compose up -d --build
```
