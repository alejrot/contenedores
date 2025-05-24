


https://podman.io/docs/installation

``` bash
sudo dnf -y install podman

sudo dnf -y install podman-machine

# opcional
sudo dnf -y install slirp4netns



sudo dnf install podman-compose.noarch
```



``` bash
podman machine init
podman machine start

```

``` bash
podman info
```



## CONSTRUIR TODO:

``` bash
podman compose up -d --build
```
