---
tags:
#   - Docker
#   - Podman
  # - MarkDown
  - Kubernetes
#   - Bash
---



# Kubernetes


Kubernetes (o K8) es un orquestador de contenedores.
El orquestador se encarga de gestionar múltiples servidores a la vez, 
a los cuales les hace ejecutar un conjunto de contenedores predefinido.

Esto permite:
- repartir el procesamiento entre varios servidores;
- dar confiabilidad a los servicios mediante la redundancia de servidores;
- gestionar los topes de recursos dedicados en cada servidor: uso de CPU, memoria RAM, etc;
- reiniciar servicios caídos de manera automática;
- etc.



Recursos recomendados: [Libros de SRE de Google](https://sre.google/books/)


## Componentes


Kubernetes es **declarativo**.
El desarrollador crea un *manifiesto* 
con el cual Kubernetes levanta un *pod*,
el cual gestiona internamente los contenedores y las réplicas.

El servicio de cluster de Kubernetes recibe el manifiesto y trata de repartir las tareas
entre los *workers*, los cuales son los servidores que ejecutarán las tareas.

El *scheduler* es el servicio que mueve los contenedores de un lugar al otro.

*kubelet* es el agente que conecta a los workers con la API del servicio de cluster. 

De forma predefinida a cada *worker* se le asigna un *pod* para ejecutar las tareas. 
Si alguno de los workers cae entonces su pod se pasará a otro worker.


