# Resumen 


- cada contenedor es una maquina virtual;
- cada imagen es como un live-CD, el cual puede ser alterado por nosotros mediante el archivo `Dockerfile`;
- cada volumen es como un servidor NAS o como un almacenamiento extraible;
- cada 'network' es una imitación de las redes privadas clase A, donde los "equipos" (los contenedores) se pueden conectar;
- los "servicios" del archivo `docker-compose.yml` no solo representan a cada contenedor por separado sino su nombre representa la IP interna del contenedor. El nombre de cada servicio sería como un "nombre de dominio", el cual puede ser usado para que unos contenedores peticionen a los otros. Por este motivo dentro del archivo Compose varios contenedores pueden reservar el mismo puerto: porque cada contenedor es visto como una PC aparte;
- El 'port mapping' funciona como el protocolo NAS en redes: ese que le permite al router de la red asignarle una IP pública a un equipo dentro de la red privada, haciendo que agentes externos a la red privada puedan "entrar". 


