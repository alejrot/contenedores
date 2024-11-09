

# Ejemplos


Este primer ejemplo conecta una rutina escrita en JavaScript con una base de datos en MongoDB.
Se trata de guardar un registro con contenido "chanchito" cada vez que se visita la URL local:

```http
http:localhost:3000/crear
```
Y los registros creados se consultarán con la URL local raíz:

```http
http:localhost:3000
```



[Repositorio de ejemplo en GitHub](https://github.com/nschurmann/mongoapp-curso-docker)



## Contenedor MongoDB

```bash 
docker create -p 27017:27017 --name monguito  -e MONGO_INITDB_ROOT_USERNAME=nico -e MONGO_INITDB_ROOT_PASSWORD=password mongo
```


```bash 
docker start monguito
```

<!-- Se probó NodeJS v18 -->

## Node Local (preliminar)

Instalar localmente NodeJS y los paquetes:
- `express` : servidor implementado en JavaScript 
- `mongoose`: conector para MongoDB implementado en JavaScript 

```bash
npm install mongoose express
```

Hay que revisar que la rutina `index.js` indique la conexión al equipo local `localhost`:

```js
mongoose.connect('mongodb://nico:password@localhost:27017/miapp?authSource=admin')
// mongoose.connect('mongodb://nico:password@monguito:27017/miapp?authSource=admin')
```



Ejecutar archivo JS.

```bash 
node index.js 
``` 

El sistema debería responder correctamente....


## Red de Docker


Se crea una red puente para poder interconectar a futuro los contenedores del proyecto.
Sólo se necesita darle un nombre:

```bash 
docker network create mired
``` 




## NodeJS en contenedor

### Corregir rutina JS
Ahora la rutina `index.js` debe conectarse al contenedor de MongoDB:

```js
// mongoose.connect('mongodb://nico:password@localhost:27017/miapp?authSource=admin')
mongoose.connect('mongodb://nico:password@monguito:27017/miapp?authSource=admin')
```


### Crear imagen con rutina JS

Se construye la imagen que contendrá tanto la rutina JS como las dependencias (NodeJS) con ayuda del archivo `Dockerfile`:

```dockerfile
FROM node:18

RUN mkdir -p /home/app

COPY . /home/app

EXPOSE 3000

CMD ["node", "/home/app/index.js"]
```

La imagen se crea con el comando `build`:


```bash 
# ruta actual del archivo Dockerfile
docker build -t miapp:v1 .
``` 


### Rehacer contenedor MongoDB

Debe incluirse la conexion a la red *bridge* del proyecto

```bash 
docker create -p 27017:27017 --name monguito --network mired  -e MONGO_INITDB_ROOT_USERNAME=nico -e MONGO_INITDB_ROOT_PASSWORD=password mongo
``` 

### Crear contenedor JS

Se crea el contenedor de la rutina JS con el mapeo IP y la referencia a la red bridge con ayuda de la imagen creada previamente

```bash 
docker create -p 3000:3000 --name chanchito --network mired   miapp:v1
``` 

### Puesta en marcha

```bash
docker start monguito chanchito
```

verificacion:

```bash
docker logs chanchito
docker logs monguito 
```


## Montar con Docker Compose

La alternativa simplificadora a los pasos previos es el uso del archivo `docker-compose.yml`.
En él se agregan las opciones de configuración, asignaciones de imágenes , construcción automática de imágenes, etc 

```yaml
# archivo 'docker-compose.yml'
version: "3.9"
services:

  # contenedor con interfaz NodeJS
  chanchito:
    build: .    # construccion de imagen - archivo Dockerfile local
    ports:
      - "3000:3000"
    links:
      - monguito
  
  # contenedor MongoDB 
  monguito:
    image: mongo    # lectura de imagen MongoDB
    ports:
      - "27017:27017"
    environment:
      - MONGO_INITDB_ROOT_USERNAME=nico
      - MONGO_INITDB_ROOT_PASSWORD=password
```

Puesta en marcha:

```bash
docker compose up
```



## Agregar volúmenes

Los volúmenes se agregan al archivo `docker-compose.yaml`

```yaml
# archivo 'docker-compose.yml'
version: "3.9"
services:

  # contenedor con interfaz NodeJS
  chanchito:
    build: .    # construccion de imagen - archivo Dockerfile local
    ports:
      - "3000:3000"
    links:
      - monguito
  
  # contenedor MongoDB 
  monguito:
    image: mongo    # lectura de imagen MongoDB
    ports:
      - "27017:27017"
    environment:
      - MONGO_INITDB_ROOT_USERNAME=nico
      - MONGO_INITDB_ROOT_PASSWORD=password
    volumes:
      # database dentro de un 'volume' 
      - mongo-data:/data/db

volumes:
  mongo-data:
```

Hace falta eliminar los viejos contenedores para poder reconstruirlos:

```bash
docker compose down
```

Puesta en marcha:

```bash
docker compose up
```

Ahora los registros creados por MongoDB son persistentes: 
ya no son reiniciados con cada nuevo container creado.



## Version desarrollo - Hot Reload


Como versión de desarrollo se reemplaza a NodeJS por Nodemon, el cual es una variante de NodeJS que permite el *hot-reload* (ejecución en modo debug, detecta cambios en la rutina JavaScript "en vivo").

Para esto crea el archivo `Dockerfile.dev`, con las instrucciones modificadas:

```dockerfile
FROM node:18

# Instalar Nodemon desde Node via NPM
RUN npm i -g nodemon        

RUN mkdir -p /home/app

WORKDIR /home/app

EXPOSE 3000

CMD ["nodemon", "index.js"]
```

Luego se arma el nuevo archivo 

```yaml
version: "3.9"
services:

  chanchito:
    build:
      context: .
      dockerfile: Dockerfile.dev
    ports:
      - "3000:3000"
    links:
      - monguito
    volumes:
      # ejecutable JS dentro de un 'volume' sin nombre 
      - .:/home/app

  monguito:
    image: mongo
    ports:
      - "27017:27017"
    environment:
      - MONGO_INITDB_ROOT_USERNAME=nico
      - MONGO_INITDB_ROOT_PASSWORD=password
    volumes:
      # database dentro de un 'volume' 
      - mongo-data:/data/db

volumes:
  mongo-data:
```

Construcción y puesta en marcha agregando la opción `-f` y el nombre de archivo YAML:

```bash
docker compose -f docker-compose-dev.yml up
```





