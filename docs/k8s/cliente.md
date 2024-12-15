





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

<!-- 

### Minikube 


[Instrucciones de instalación](https://minikube.sigs.k8s.io/docs/start/)

```bash
choco install minikube
```
```bash
minikube start
```
 -->



## Proveedores de Cloud

Proveedores de Cloud para Kubernetes:

DigitalOcean, AWS, GoogleCloud, Linode, etc.


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



<!-- SALTO DE CONTENIDOS AQUI -->





## logs y stern

```yaml
kubectl logs -f nombre_completo_pod
```



stern requiere instalación

```yaml
stern parte_nombre
```


## Base64

```bash
echo admin | base64   # 'YWRtaW4K'
echo c3VwM3JwYXNzdzByZA== | base64 -d      # 'sup3rpassw0rd'
```



## Kubesealed

[Kubesealed - Cifrando secretos en Kubernetes](https://www.youtube.com/watch?v=YuKAiy0Olc0)


## Helm


[HELM: INSTALÁ WORDPRESS con 1 COMANDO en KUBERNETES](https://youtu.be/CPjfb-I_BKU)


[GitHub de Helm](https://github.com/helm/helm/releases)


## Referencias


[Pelado Nerd - KUBERNETES De NOVATO a PRO! (CURSO COMPLETO EN ESPAÑOL)](https://www.youtube.com/watch?v=DCoBcpOA7W4)