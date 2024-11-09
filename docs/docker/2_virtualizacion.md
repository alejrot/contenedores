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

# Virtualización


## Imagen
Es un empaquetado con las dependencias, el código ,variables de entorno, etc
Cada imagen tiene un software distinto: sistema operativo invitado, biblioteca, framework, etc. Distintas versiones del software elegido implican distintas imágenes.
Sin embargo, las imágenes
están divididas internamente en capas (*layers*), las cuales pueden ser reutilizadas por múltiples imágenes. 
Esto ayuda a ahorrar espacio en disco evitando el duplicado de capas repetidas. 


## Contenedor
Es una apilado de imágenes capaz de ser ejecutado.
La capa inferior del contenedor es el kernel elegido. Es muy popular usar Alpine Linux por su pequeño tamaño.


### Docker vs Virtual Machines (VM)

El problema puede pensarse en tres capas

- Aplicaciones
- Kernel
- Hardware


Las máquinas virtuales instalan tanto el kernel como las aplicaciones, demandando muchos GB de espacio y exigiendo esfuerzo computacional extra.

Los gestores de contenedores como Docker trabajan compartiendo un kernel para todas las aplicaciones, minimizando el software redundante.


### Modos virtualización:

- **Para virtualización:** 
el kernel anfitrión (*host*) da soporte al kernel invitado (*guest*) de cada máquina virtual. 
Esta virtualización intenta dar tanto  acceso a los kernels invitados al hardware como sea posible.
- **Virtualizacion Parcial:** 
algunos componentes del hardware se virtualizan para el SO huésped, haciendo que éste no pueda acceder a ellos directamente.
- **Virtualizacion Completa:** 
todos los recursos de hardware son virtualizados, ergo los kernels invitados no tienen acceso directo.

Docker usa el kernel anfitrión directamente:

- Las imágenes pesan mucho menos que las máquinas virtuales;
- Los contenedores arrancan casi de inmediato;
- El rendimiento es mucho mejor.

