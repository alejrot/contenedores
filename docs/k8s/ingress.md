---
tags:
#   - Docker
#   - Podman
  # - MarkDown
  - Kubernetes
#   - Bash
---


# Ingress

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


