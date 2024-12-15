---
tags:
#   - Docker
#   - Podman
  # - MarkDown
  - Kubernetes
#   - Bash
---



# Servicios


- Cluster IP
- Node Port
- Load Balancer

## Cluster IP

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

## Node Port


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


## Load Balancer


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
