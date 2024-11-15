

# Configuración de Docker: WSL

El WSL (Windows Subsystem for Linux) es el soporte para los containers de Docker.
Hay dos versiones de WSL disponibles:

- la versión 1 que viene activada por defecto;
- la versión 2 que tiene algunas mejoras funcionales y mejor rendimiento. 

A cada una se le puede asignar una imagen de kernel, 
sea de una distribución Linux 
(las más habituales son Alpine, Debian y Ubuntu) 
o incluso Windows. 
La distribución más popular es Alpine Linux por sus bajos requisitos de memoria y de espacio en disco, 
además de su gran velocidad de ejecución.
[Kernel Alpine descargable desde GitHub](https://github.com/yuk7/AlpineWSL/releases/tag/3.16.0-0)

## [¿Qué distros tengo en WSL? (Win 10)](https://terminaldelinux.com/terminal/wsl/instalacion-configuracion-wsl/#qué-distros-tengo-en-wsl)


Enumerar qué distribuciones están instaladas y cuál lo está por defecto:

```bash
wsl --list
```

Enumerar distribuciones disponibles online:

```bash
wsl --list --online
```



Si no hay ninguna actúa una imagen predeterminada de Docker llamada docker-desktop, la cual tiene un importante consumo de recursos.

Arranca la distribución elegida:
```bash
wsl -d 		<nombre_distro>
```

Elegimos qué distribución es la que queremos usar por defecto.
```bash
wsl --set-default  	<nombre_distro>
wsl -s 			<nombre_distro>
```
Indica qué versión de WSL hace funcionar a cada kernel instalado y si está corriendo o no:
```bash
wsl --list --verbose
```

Elige qué versión de WSL ejecuta el kernel elegido:

```bash
wsl --set-version <nombre_distro> <numero_version>
```

**HINT:** 
ejecutar este comando con la terminal ubicada en la direccion del kernel añadido, 
esto es para prevenir errores tipo “el archivo del kernel no debe estar comprimido” etc.

Eligé qué version de WSL ejecutar habitualmente (se recomienda la 2):

```bash
wsl --set-default-version <numero_version>
```
Reinicio del subsistema de Linux:
```bash
wsl --shutdown
```

Para eliminar una distribución de linux del WSL se puede hacer:

```bash
wsl --unregister  distribucion
```


https://www.windowscentral.com/how-completely-remove-linux-distro-wsl


```bash
wsl --import <distribucion> <direccion_ instalacion>
```


Configuración en Windows: `wsl.conf` y `.wslconfig`

- `wsl.conf` → Archivo en la distribución Linux / Docker usada. Ubicacion: `/etc/wsl.conf`
- `.wslconfig` → Archivo para Windows que va en la carpeta de usuario (si no existe se crea)

