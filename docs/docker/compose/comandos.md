

# comandos
 



## Ejecución

### Creación automática



Crea los contenedores deseados y las imágenes necesarias en base al archivo `docker-compose.yml`. 
```bash
docker compose up
docker-compose up		# versiones antiguas de Docker
```

También crea las redes necesarias por defecto.
Una vez terminado los hace funcionar. 
Puede interrumpirse haciendo  ++ctrl+c++ .

### Eliminación automática


Eliminar los contenedores diseñados con el archivo `docker-compose.yml`:
```bash
docker compose down
docker-compose down		# versiones antiguas de Docker
```
También elimina las redes comunes entre contenedores.


## Versión de desarrollo


### Creación automática


```bash
docker compose -f docker-compose-dev.yml up
```


### Eliminación automática 


```bash
docker compose -f docker-compose-dev.yml  down
```
