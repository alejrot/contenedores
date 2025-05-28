# Variables de entorno


Los archivos Compose admiten el uso de variables de entorno
para configurar distintos parámetros de las imágenes usadas,
elegir valored de despliegue de los contenedores, etc.


## Variables generales

### Archivo `.env`

El comando Compose busca de manera predefinida
por archivos `.env` (archivos ocultos)
en el directorio del archivo `compose.yml`
y busca en su interior por definiciones de variables de entorno. 

Tómese por ejemplo esta estructura de archivos:

```bash title="Estructura de archivos"
.
├── compose.yml
└── .env
```

En el archivo de entorno se definen las variables de entorno de interés
y sus valores.
En este ejemplo se define una única variable  llamada `PUERTO`:

```bash title="Archivo .env"
PUERTO=9999
```

En el archivo `compose.yml` se intenta obtener el valor la variable de entorno
dentro del comando de Bash especificado:


```yaml title="Archivo Compose"
name: "entorno_default"

services:

  valor_variable:
    image: busybox:1.37.0-glibc
    command: echo "La variable PUERTO vale '${PUERTO}'"
```

entonces el reporte al ser desplegado es algo como:

``` title="Compose - reporte"
[valor_variable] | La variable PUERTO vale '9999'
```

### Variables en *shell*

El comando `compose` es sensible a valores de variables definidas desde la misma terminal.
Por ejemplo, si en el ejemplo previo
se crea la variable `PUERTO`
con el valor `7777` desde la *shell*:


```bash title="Bash - Crear variable de entorno"
export PUERTO=7777
```

Al repetir el despliegue el valor leído será el asignado desde la *shell*:

```  title="Compose - reporte"
[valor_variable] | La variable PUERTO vale '7777'
```

la variable de entorno se borra de la *shell* con el comando `unset`

```bash title="Bash - Eliminar variable de entorno"
unset PUERTO
```

!!! tip "Jerarquía de definiciones"

    Si la variable de entorno está definida en la terminal y en archivo
    entonces se toma el valor definido en la terminal.
    

## Variables específicas de imágenes

Las imágenes de los contenedores
suelen tener sus propias variables de entorno,
las cuales se usan para la configuración interna. 
Estas variables son especificadas


### Asignación manual


Los valores fijos son la forma más simple de asignar los valores
de las variables de entorno:

```yaml title="Variables de imagen - Asignación manual"
services:

  contenedor:
    image: nombre_imagen
    environment:
      - VARIABLE_1: valor_1
      - VARIABLE_2: valor_2
      - VARIABLE_3: valor_3
```

### Asignación con variables de entorno

Las variables de entorno del sistema anfitrión
pueden ser asignadas a las variables de entorno de las imágenes usadas:

```yaml title="Variables de imagen - Con variables de entorno"
services:

  contenedor:
    image: nombre_imagen
    environment:
      - VARIABLE_1: ${variable_1}
      - VARIABLE_2: ${variable_2}
      - VARIABLE_3: ${variable_3}
```


### Lectura desde archivo

A cada contenedor se le puede definir una lista de rutas de archivo
con variables de entorno guardadas:

```yaml title="Variables de imagen - desde archivo (básico)"
services:

  contenedor:
    image: nombre_imagen
    env_file:
      - ruta_1.env
      - ruta_2.env
```

Una sintaxis más completa consiste en
indicar la lista desglosada en sus parámetros internos:

- `path`: indica la ruta de archivo;
- `required`: define si el archivo es obligatorio (`true` por default)
- `format`: define el formato de especificaciín de las variables.


```yaml title="Variables de imagen - desde archivo (completa)"
services:

  contenedor:
    image: nombre_imagen
    env_file:
      - path: ./ruta_1.env
        required: true    # default
      - path: ./ruta_2.env
        required: false
        format: raw       # default -> raw: clave=valor
```


## Interpolación

La interpolación es la lectura de variables de entorno para asignar valores a otras variables.
En los archivos Compose se aceptan varias opciones que son calcadas de la notación definida en Bash.


!!! info "BASH - Resumen"

    ```bash title="Creacion "
    VARIABLE            # variable vacía
    VARIABLE=           # variable vacía
    VARIABLE=valor      # variable con valor
    ```

    ```bash title="Creacion - Variables de entorno"
    export VARIABLE            # variable vacía
    export VARIABLE=           # variable vacía
    export VARIABLE=valor      # variable con valor
    ```

    ```bash title="Lectura y asignación"
    VALOR=${VARIABLE}   # asignación
    echo ${VARIABLE}    # muestra en pantalla
    ```

    ```bash title="Elimninación"
    unset VARIABLE      # eliminación
    ```



### Sustitución directa:

En este caso se intenta leer la variable y se deja su valor tal como está:

```bash
${VARIABLE}
```

Si la variable no existe entonces queda un *string* vacío.


### Valor por default

El valor por default sirve para autocompletar
el valor de la variable
en caso que este falte.
Usa el signo `-`:

```bash title="valor por default"
${VARIABLE:-default}  # uso: variables vacías e inexistentes
${VARIABLE-default}   # uso: sólo variables inexistentes
```


### Valor requerido

Se puede disparar una respuesta de error mediante el simbolo `?`:

```bash
${VARIABLE:?error}  # error: variables vacías e inexistentes
${VARIABLE?error}   # error: sólo variables inexistentes
```


### Valor de reemplazo

Esta opción sustituye el valor de la variable de entrada
por el valor predefinido.
Usa el valor `+`: 

```bash
${VARIABLE:+reemplazo}  # sustitucion: variables con valor
${VARIABLE+reemplazo}   # sustitucion: variables existentes
```

Si la condición no se cumple se deja vacía.


