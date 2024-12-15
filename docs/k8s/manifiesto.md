---
tags:
#   - Docker
#   - Podman
  # - MarkDown
  - Kubernetes
#   - Bash
---


# Manifiestos (recursos)

## Namespaces

Son divisiones lógicas del cluster de Kubernetes.
Separan la carga pero no impiden la lectura de datos de uno a otro.

```bash
kubectl get ns
```

|NAME             | STATUS  | AGE|
|---|---|---|
|`default`          | Active  | 37m|
|`kube-node-lease ` | Active  | 37m|
|`kube-public`      | Active  | 37m|
|`kube-system`     | Active  | 37m|


Los *namespaces* que comienzan con `kube` son reservados para las herramientas de Kubernetes. 
<!-- e incluyen información de la -->

## Pods

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

el comando indica estado del pod, tiempo de funcionamiento, veces que se reinició, etc.


Se muestra más información agregando `-o wide`: IP , nodo, etc.
```bash
kubectl -n kube-system get pods -o wide
```

Cierre manual de un pod particular
```bash
kubectl -n nombre_espacio delete pod nombre_pod 
```



## Crear pod


Recurso recomendado: [manifiestos del Pelado Nerd en GitHub](https://github.com/pablokbs/peladonerd/tree/master/kubernetes/35)


Creamos un manifiesto llamado `01-pod.yaml`

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:alpine
    ports:
        - containerPort: 80
```

Cada tipo de pod requiere una versión especifica de API.


El manifiesto se ejecuta con el comando `apply`:

```bash
kubectl apply -f 01-pod.yaml
```
y se verifica el funcionamiento con `get pods`:

```bash
kubectl get pods
```

El pod construido en este caso se llama `nginx`. 
Al no haber indicado un *namespace* el pod se creará en *default*.

Para abrir una terminal interactiva adentro del pod se usa el comando `exec`:

```bash
kubectl exec -it nginx -- sh
```
El acceso es otorgado por la API de Kubernetes.


El pod se elimina con `delete`

```bash
kubectl delete pod nginx
```


## Variables de entorno

```yaml
# ...
spec:
  containers:
    # ...   
    env:
    - name: MI_VARIABLE
      value: "pelado"
    - name: MI_OTRA_VARIABLE
      value: "pelade"
    - name: DD_AGENT_HOST
      valueFrom:
        fieldRef:
          fieldPath: status.hostIP
```

## Recursos


Con el campo `resources` se especifican tanto los recursos garantizados a cada pod (`requests`) 
como los recursos límite a los que pueden acceder entre todos ellos (`limits`).

```yaml
# ...
spec:
  containers:
    # ...   
    resources:
      requests:
        memory: "64Mi"      # memoria en megabytes
        cpu: "200m"         # demanda de cores en milésimas
      limits:
        memory: "128Mi"     # memoria en megabytes
        cpu: "500m"         # demanda de cores en milésimas
```

`cpu` es el número de *cores* (núcleos) y se especifica habitualmente en milésimas. 
`memory` es la memoria RAM disponible y se especifica normalmente en MiBs.
En este ejemplo, 
`cpu:"200m"` indica que cada pod tiene garantizado el 20% del tiempo de uso de un *core*
en tanto que el conjunto de *pods* disponen de hasta el 50% del tiempo de un núcleo (`cpu: "500m"`).
Por ello un mismo worker podría ejecutar en simultáneo hasta 2 *pods* definidos por usuario.

Debe señalarse que en el límite impuesto a los recursos incluye también a los pods del sistema.

Si algún pod intenta excederse en su consumo de RAM 
entonces el kernel de Linux cerrará dicho pod automáticamente 
e intentará crear uno nuevo.

En cambio, si algún pod intenta superar la demanda de CPU máxima habilitada
entonces el kernel de Linux lo "ahogará" (*CPU throttling*) para reducir su demanda.


## Ensayos

Kubernetes monitorea los pods para verificar que estos funcionen.
Para ellos habilita dos ensayos: 
uno de arranque (`readinessProbe`) 
y otro de disponibilidad (`livenessProbe`).

Cada ensayo es configurado por separado:

```yaml
# ...
spec:
  containers:
    # ...   
    readinessProbe:
      httpGet:
        path: /
        port: 80
      initialDelaySeconds: 5
      periodSeconds: 10
    livenessProbe:
      tcpSocket:
        port: 80
      initialDelaySeconds: 15
      periodSeconds: 20
```
y habilita a elegir el protocolo a usar, rutas, número de puertos, etc.

Debido a que los *pods* suelen requerir un tiempo apreciable para ponerse en marcha
es conveniente definir un retardo inicial para los ensayos (`initialDelaySeconds`)
además del tiempo entre testeos (`periodSeconds`)


## Leer manifiesto

El manifiesto de un pod preexistente se consulta así:

```bash
kubectl get pod nginx -o yaml
```
