# Ejercicio 5

## HEALTCHECK

> Ref: https://docs.docker.com/engine/reference/builder/#healthcheck

### ¿Qué hace?

Directiva para definir un comando que pueda determinar si el contenedor está funcionando correctamente.

Sintaxis:

    HEALTHCHECK [options] CMD <command>

Las opciones determinan condiciones para repetir la ejecución del comando: cuántas veces, cada cuánto tiempo, etc.

El comando debe retornar `0` o `1` de acuerdo a si lo que queremos comprobar se verifica o no, respectivamente.

Al usar esta directiva, el estado de salud de un contendeor se puede ver en la columna `STATUS` al ejecutar `docker ps`. Se podrá ver la variable `health` que puede tomar uno de estos 3 valores:

* `starting`: El comando nunca retornó `0` y las condiciones indican que debe ejecutarse al menos una vez más.

* `healthy`: El comando retornó `0` en una ejecución (no se ejecuta más en este caso, independientemente de las condiciones).

* `unhealthy`: El comando siempre retornó `1` y ya se agotaron todas las condiciones para volver a correrlo.

### ¿Para qué se usa?

* Poder detectar un estado anómalo al iniciar un contenedor
* Para dependencia de contenedores en `docker-compose`: esperar a que el contenedor del que se depende esté listo con `condition: service_healthy`.

## ONBUILD

> Ref: https://docs.docker.com/engine/reference/builder/#onbuild

### ¿Qué hace?

Registrar una directiva para que sea ejecutada cuando la imágen actual sea usada como base de otra imagen.

Sintaxis:

    ONBUILD <INSTRUCTION>

### ¿Para qué se usa?

Por ej. se puede crear un Dockerfile que ejecute una aplicación a partir de código fuente diferente.
En este caso, se puede usar `ONBUILD` para que la copia del código fuente se ejecute en un paso intermedio cuando
la imagen sea usada como base de otra imagen.

## VOLUME

> Ref: https://docs.docker.com/engine/reference/builder/#volume

### ¿Qué hace?

Directiva para crear un mapeo de un directorio del container a un volumen administrado por docker para persistir datos.

Sintaxis:

    VOLUME ["/nombre"]

Estos volumenes se pueden administrar con `docker volume`, listar con `docker volume ls` y en linux por ej. se encuentran en `/var/lib/docker/volumes/`.

### ¿Para qué se usa?

* Para persistir datos generados en un container.
* Para compartir datos entre containers.
