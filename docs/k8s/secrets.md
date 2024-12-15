---
tags:
#   - Docker
#   - Podman
  # - MarkDown
  - Kubernetes
#   - Bash
---



# Secrets

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