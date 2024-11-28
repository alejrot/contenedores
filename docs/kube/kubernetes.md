

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


### Minikube 


[Instrucciones de instalación](https://minikube.sigs.k8s.io/docs/start/)

```bash
choco install minikube
```
```bash
minikube start
```

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

|NAME             | STATUS  | AGE|
|---|---|---|
|`default`          | Active  | 37m|
|`kube-node-lease ` | Active  | 37m|
|`kube-public`      | Active  | 37m|
|`kube-system`     | Active  | 37m|


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

el comando indica estado del pod, tiempo de funcionamiento, veces que se reinició, etc.


Se muestra más información agregando `-o wide`: IP , nodo, etc.
```bash
kubectl -n kube-system get pods -o wide
```

Cierre manual de un pod particular
```bash
kubectl -n nombre_espacio delete pod nombre_pod 
```
<!-- 

El pod cerrado debe reiniciarse automáticamente, aunque con un nombre ligeramente distinto.
 -->


### Crear pod


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


### variables de entorno

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

### recursos


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


### Ensayos

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


### leer manifiesto

El manifiesto de un pod preexistente se consulta así:

```bash
kubectl get pod nginx -o yaml
```


## Deployment

El manifiesto del tipo `Deployment` permite a Kubernetes
poner en marcha y controlar a múltiples pods iguales llamados *réplicas*.


```yaml
# archivo '04-deployment.yaml'
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:               # especificaciones del despliegue
  selector:
    matchLabels:
      app: nginx
  replicas: 2       # n1 de contenedores iguales internos
  template:
    metadata:
      labels:
        app: nginx
    spec:           # especificaciones de los containers   
      containers:
        #....       # configuracion de containers
```

La configuración de los contenedores internos es igual a la usada con los pods.

Si alguno de los pods creado es eliminado se creará uno nuevo automáticamente.


## DaemonSet

Los pods del tipo `DaemonSet` son desplegados en todos los nodos, uno por nodo.
Sirven para hacer monitoreo.


```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        # ...
```

## StatefulSet

Estos manifiestos incluyen un volumen persistente. 
De esta manera los datos no se pierden si alguno de los pods es eliminado.

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: my-csi-app-set
spec:
  selector:
    matchLabels:
      app: mypod
  serviceName: "my-frontend"
  replicas: 1
  template:
    metadata:
      labels:
        app: mypod
    spec:
      containers:
      - name: my-frontend
        image: busybox
        args:
        - sleep
        - infinity
        volumeMounts:
        - mountPath: "/data"
          name: csi-pvc
  volumeClaimTemplates:
  - metadata:
      name: csi-pvc
    spec:
      accessModes:
      - ReadWriteOnce
      resources:
        requests:
          storage: 5Gi      # 5GiB de espacio en disco
      storageClassName: do-block-storage
```

Kubernetes crea en el servidor remoto el volumen requerido de manera automática.


Nuevo comando: `describe`

```bash
kubectl describe pod nombre_pod
```

PVC: Persistent Volume Claim

Estos volumenes se listan con:

```bash
kubectl get pvc
```


Los manifiestos del tipo `StatefulSet` tienen su propia opcíon de listado:

```bash
kubectl get statefulsets
kubectl get sts
```

Eliminacion:
```bash
kubectl delete sts nombre_sts
```

```bash
kubectl delete pvc nombre_pvc
```






## Networking






## Servicios


- Cluster IP
- Node Port
- Load Balancer

### Cluster IP

En este manifiesto se crean un *deployment* y un servicio que
crea una dirección IP privada con la cual se puede acceder a los pods internos.
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello
spec:
  replicas: 3
  selector:
    matchLabels:
      role: hello
  template:
    metadata:
      labels:
        role: hello
    spec:
      containers:
      - name: hello
        image: gcr.io/google-samples/hello-app:1.0
        ports:
        - containerPort: 8080

---
apiVersion: v1
kind: Service
metadata:
  name: hello
spec:
  ports:
  - port: 8080
    targetPort: 8080
  selector:
    role: hello
```

Los puertos de los contenedores internos van variando según son creados. 
El servicio va buscando y conectando a esos contenedores internos.
El tipo por defecto de los servicios es `CLusterIP`, por eso no se indica.

### Node Port


Este servicio expone un puerto especificado para cada nodo.
Por lo demás es muy similar al servicio previo.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello
spec:
  replicas: 3
  selector:
    matchLabels:
      role: hello
  template:
    metadata:
      labels:
        role: hello
    spec:
      containers:
      - name: hello
        image: gcr.io/google-samples/hello-app:1.0
        ports:
        - containerPort: 8080

---
apiVersion: v1
kind: Service
metadata:
  name: hello
spec:
  type: NodePort            # tipo explicito
  ports:
  - port: 8080
    targetPort: 8080
    nodePort: 30000
  selector:
    role: hello
```


### Load Balancer


```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello
spec:
  replicas: 3
  selector:
    matchLabels:
      role: hello
  template:
    metadata:
      labels:
        role: hello
    spec:
      containers:
      - name: hello
        image: gcr.io/google-samples/hello-app:1.0
        ports:
        - containerPort: 8080

---
apiVersion: v1
kind: Service
metadata:
  name: hello
spec:
  type: LoadBalancer        # tipo explicito
  ports:
  - port: 8080
    targetPort: 8080
  selector:
    role: hello
```


## Ingress

`Ingress` permite el acceso a los servicios mediante el path de la URL.
Kubernetes despliega un controlador NGINX dentro del clúster, 
el cual se va a autoconfigurar para redireccionar el tráfico a los servicios.

El controlador ingress necesita ser instalado para su uso.

Recomendado: Helm

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: hello-app
spec:
  rules:
  - http:
      paths:
      - path: /v1
        pathType: Prefix
        backend:
          service:
            name: hello-v1
            port:
              number: 8080
      - path: /v2
        pathType: Prefix
        backend:
          service:
            name: hello-v2
            port:
              number: 8080
```


Los manifiestos ingress se consultan con su propia opción:

```yaml
kubectl get ing
```

El controlador Ingress suele crear tambíén un load balancer y configura la dirección IP

```yaml
kubectl -n ingress-nginx get svc
```


Una vez creado el manifiesto *ingress* el funcionamiento se puede verificar con `curl`:

```yaml
curl http://dirección_ip/v1
curl http://dirección_ip/v2
```


## ConfigMap

El configMap es un archivo que se sube al servidor Kubernetes y al que los pods tendrán acceso.



```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: game-demo
data:
  # property-like keys; each key maps to a simple value
  player_initial_lives: "3"
  ui_properties_file_name: "user-interface.properties"
  #
  # file-like keys
  game.properties: |
    enemy.types=aliens,monsters
    player.maximum-lives=5
  user-interface.properties: |
    color.good=purple
    color.bad=yellow
    allow.textmode=true
```




```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
    - name: nginx
      image: nginx:alpine
      env:
        # Define the environment variable
        - name: PLAYER_INITIAL_LIVES # Nombre de la variable
          valueFrom:
            configMapKeyRef:
              name: game-demo           # El confimap desde donde vienen los valores
              key: player_initial_lives # La key que vamos a usar
        - name: UI_PROPERTIES_FILE_NAME
          valueFrom:
            configMapKeyRef:
              name: game-demo
              key: ui_properties_file_name
      volumeMounts:
      - name: config
        mountPath: "/config"
        readOnly: true
  volumes:
    - name: config
      configMap:
        name: game-demo # el nombre del configmap que queremos montar
        items: # Un arreglo de keys del configmap para crear como archivos
        - key: "game.properties"
          path: "game.properties"
        - key: "user-interface.properties"
          path: "user-interface.properties"
```





## Secrets

Los los valores secretos van codificados en base64



```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-credentials
type: Opaque
data: 
  username: YWRtaW4=                # 'admin'
  password: c3VwM3JwYXNzdzByZA==    # 'sup3rpassw0rd'

# Esto se puede crear a mano:
# kubectl create secret generic db-credentials --from-literal=username=admin --from-literal=password=sup3rpassw0rd
# Docs: https://kubernetes.io/es/docs/concepts/configuration/secret/
```


```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:alpine
    env:
    - name: MI_VARIABLE
      value: "pelado"
    - name: MYSQL_USER
      valueFrom:
        secretKeyRef:
          name: db-credentials
          key: username
    - name: MYSQL_PASSWORD
      valueFrom:
        secretKeyRef:
          name: db-credentials
          key: password
    ports:
    - containerPort: 80
```

## Kustomization


```yaml
apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization

commonLabels:
  app: ejemplo

resources:
- 15-pod-secret.yaml

secretGenerator:
- name: db-credentials
  literals:
  - username=admin
  - password=secr3tpassw0rd!

images:
- name: nginx
  newTag: latest
```

## logs y stern

```yaml
kubectl logs -f nombre_completo_pod
```



stern requiere instalación

```yaml
stern parte_nombre
```



## Referencias


[Pelado Nerd - KUBERNETES De NOVATO a PRO! (CURSO COMPLETO EN ESPAÑOL)](https://www.youtube.com/watch?v=DCoBcpOA7W4)