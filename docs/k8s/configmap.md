---
tags:
#   - Docker
#   - Podman
  # - MarkDown
  - Kubernetes
#   - Bash
---



# ConfigMap

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

