---
tags:
#   - HTML5
  # - JavaScript
  # - CSS
#   - YAML
#   - MkDocs
#   - Python
  - Docker
#   - Podman
  # - MarkDown
#   - TypeScript
  # - CSV
#   - Bash
#   - Express
#   - ReactJS
#   - NodeJS
#   - NPM
#   - PNPM
#   - ViteJS
#   - SQLite
  # - SQLAlchemy
  # - MySQL
  # - PostgreSQL
  # - MariaDB
---





# Docker




[Aprende Docker ahora! curso completo gratis desde cero!](https://www.youtube.com/watch?v=4Dko5W96WHg)


[Repositorio de ejemplo](https://github.com/nschurmann/mongoapp-curso-docker)


[Manuales oficiales de Docker](https://docs.docker.com/reference/)

## Introducción


Docker es un administrador de **contenedores de software**. 
Los contenedores facilitan el desarrollar,
compartir y poner en marcha el nuevo software minimizando los problemas de dependencias, 
actualizaciones etc. 
Los contenedores son una versión simplificada de las máquinas virtuales, 
permitiendo la ejecución de un kernel (sistema operativo) y software adicional (frameworks, bibliotecas, etc )
dentro de un sistema con otro sistema operativo. 

## Despliegue sin contenedores

Requiere:

- código
- lista de dependencias 
- archivos de configuración

Problemas:

- Distintas versiones de dependencias
- múltiples modos de instalación
- múltiples requisitos de instalación según sistema operativo
- Problemas de comunicación
- Los problemas se detectan al desplegar → requerirá una vuelta atrás (*rollback*)

## Despliegue con contenedores

El equipo de desarrollo y el de operaciones crean en conjunto una imagen. 
La imagen depende del *runtime* de Docker. 
El proceso puede automatizarse con los pipelines  de los servicios de controladores de versión (github, BitBucket, etc).

