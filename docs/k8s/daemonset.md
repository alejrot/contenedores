---
tags:
#   - Docker
#   - Podman
  # - MarkDown
  - Kubernetes
#   - Bash
---



# DaemonSet

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

