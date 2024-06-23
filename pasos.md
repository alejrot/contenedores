https://www.redhat.com/sysadmin/podman-docker-compose


# `docker compose` en Podman



instalar `podman-docker` y `docker-compose`


Arrancar servicio Podman:

    sudo systemctl start podman.socket

Verificar conexion:

    sudo curl -H "Content-Type: application/json" --unix-socket /var/run/docker.sock http://localhost/_ping



