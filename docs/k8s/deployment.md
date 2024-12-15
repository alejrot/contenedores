---
tags:
#   - Docker
#   - Podman
  # - MarkDown
  - Kubernetes
#   - Bash
---



# Deployment

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


