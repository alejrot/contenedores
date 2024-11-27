

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


## Componentes



## Proveedores

Proveedores de Cloud para Kubernetes:

DigitalOcean, AWS, GoogleCloud, Linode, etc.


## Kubectl

Es el cliente de Kubernetes

Consulta de versión (local):

```bash
kubectl version --client=true
```

Si se omite la opción `--client=true` entonces el cliente intentará descargar la versión del clod actual de Kubernetes y dará error.


## Instalar cluster 

- Docker Desktop
- Kind : utilidad para crear clusters locales con fines de desarrollo y testeo. [página de Kind](https://kind.sigs.k8s.io/)
- Minikube: crea una máquina virtual para correr Kubernetes. Acepta plugins.

## Conexion remota

En caso de usar un servicio de cloud hay que descargar un archivo de configuraciones, credenciales, etc. desde la página web del servicio.
Luego se crea una variable de entorno con la ruta al archivo de configuración:

```bash
export KUBECONFIG=ruta_archivo_config
```

por ejemmplo si el archivo está en la carpeta de descargas de usuario:
```bash
export KUBECONFIG=~/Downloads/nombre_archivo
```
y la conexión se hace con el comando `get nodes`
```bash
kubectl get nodes
```




## Comandos

....

Ver contextos:
```bash
kubectl  config get-contexts
```

Recomendado: [Lens (IDE para Kubernetes)](https://k8slens.dev)


## Manifiestos (recursos)

### Namespaces

Son divisiones lógicas del cluster de Kubernetes.
Separan la carga pero no impiden la lectura de datos de uno a otro.

```bash
kubectl get ns
```

- `default`
- `kube-node-lease`
- `kube-public`
- `kube-system`

Los *namespaces* que comienzan con `kube` son reservados para las herramientas de Kubernetes. 
<!-- e incluyen información de la -->

### pods

Un *pod* es un conjunto de contenedores. 
Se recomienda ejecutar un proceso por contenedor, 
por ello de ejecutarse varios procesos juntos conviene asignar uno por contenedor
y agrupar los contenedores mediante un pod.
De esta forma Kubernetes monitoreará y controlará automáticamente a todos los *containers* internos,
cada uno con sus propias reglas.

Consulta de pods para un namespace particular: `get pods`
```bash
kubectl -n nombre_namespace get pods
```

ejemplo: namespace `kube-system`
```bash
kubectl -n kube-system get pods
```
Se meustra más información agregando `-o wide`
```bash
kubectl -n kube-system get pods -o wide
```







## Referencias


[Pelado Nerd - KUBERNETES De NOVATO a PRO! (CURSO COMPLETO EN ESPAÑOL)](https://www.youtube.com/watch?v=DCoBcpOA7W4)