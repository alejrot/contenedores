

# Contenedores

Los contenedores facilitan el desarrollar,
compartir y poner en marcha el nuevo software minimizando los problemas de dependencias, 
actualizaciones etc. 
Los contenedores son una versión simplificada de las máquinas virtuales, 
permitiendo la ejecución de un kernel (sistema operativo) y software adicional (frameworks, bibliotecas, etc )
dentro de un sistema con otro sistema operativo. 

El uso de contenedores mejora la confiabilidad de los despliegues de software en servidores 
e incluso en entornos locales, 
al permitir una gestión más simple del entorno de ejecución
y prevenir conflictos posibles entre distintas versiones de las dependencias.


## Docker y Podman

Los administradores de contenedores permiten crear y desplegar los contenedores,
como asi también gestionar, modificar y publicar las imágenes requeridas para dichos contenedores.

El administrador de contenedores más popular es **Docker**, el cual es de caracter privativo.
Una alternativa de código abierto pensada para una alta compatibilidad con Docker es **Podman**.
Ambas opciones son los usados de referencia para esta documentación.


[**Comenzar con Docker y Podman**](docker/01_intro.md)



## Kubernetes

Los orquestadores de contenedores son programas que permiten replicar 
y controlar múltiples conjuntos de contenedores iguales de manera automática,
repartiendo el procesamiento entre múltiples servidores a los que controla.

Esto permite:

- repartir la carga de trabajo entre múltiples dispositivos físicos;
- aumentar la confiabilidad de los servicios mediante la redundancia de servidores;
- gestionar los topes de recursos dedicados en cada servidor: uso de CPU, memoria RAM, etc;
- reiniciar servicios caídos de manera automática;
- etc.

El orquestador más popular del momento es **Kubernetes**, tambnién llamado **K8s**.

[**Comenzar con Kubernetes**](k8s/intro.md)