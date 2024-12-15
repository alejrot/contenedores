---
tags:
#   - Docker
#   - Podman
  # - MarkDown
  - Kubernetes
#   - Bash
---



# StatefulSet


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

