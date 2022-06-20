# Ejercicio 7

1. Verificar que la aplicación levantó correctamente:

    <img src="app-ok.png" width="300">

2. Se están ejecutando 2 contenedores:

    ```shell
    $ docker ps
    CONTAINER ID   IMAGE                            COMMAND                  CREATED         STATUS         PORTS                    NAMES
    28e25799cc96   nicopaez/jobvacancy-ruby:1.3.0   "/jobvacancy/start_a…"   3 minutes ago   Up 3 minutes   0.0.0.0:3000->3000/tcp   ejercicio07_web_1
    0596980d6daa   postgres                         "docker-entrypoint.s…"   3 minutes ago   Up 3 minutes   5432/tcp                 ejercicio07_db_1
    ```

2. Los contenedores se basan en las imagenes `nicopaez/jobvacancy-ruby:1.3.0` y `postgres`.

3. Detalle de las instrucciones:

    ```yaml
    # versión de la sintaxis de compose utilizada
    version: '2'
    # definir servicios a correr en cada contenedor
    services:
    # definir el servicio "web"
    web:
        # imagen base del contenedor
        image: nicopaez/jobvacancy-ruby:1.3.0
        # definir hostnames para comunicase con un servicio en otro container,
        # este está igual al comportamiento por defecto, también podría ser: `db:otro_nombre`.
        links:
        - db
        # mapear puertos host:contenedor.
        ports:
        - "3000:3000"
        # definir variables de entorno
        environment:
        # definir valor de variable de entorno "PORT"
        PORT: "3000"
        # definir valor de variable de entorno "RACK_ENV"
        RACK_ENV: "production"
        # definir valor de variable de entorno "DATABASE_URL"
        # (notar que se usa el hostname definido en links)
        DATABASE_URL: "postgres://postgres:Passw0rd!@db:5432/postgres"
        # iniciar el servicio "db" antes que este cuando se ejecute "docker-compose up".
        depends_on:
        - db
    # definir el servicio "web"
    db:
        # imagen base del contenedor
        image: postgres
        # definir variables de entorno
        environment:
        # definir valor de variable de entorno "POSTGRES_PASSWORD"
        POSTGRES_PASSWORD: Passw0rd!

    ```

4. Por defecto, los contenedores corren en la misma red, y cada contenedor puede acceder a otro contenedor a través del nombre del servicio definido en `services`. A través del parámetro `links` se puede cambiar el nombre mediante el cual se accede a un servicio corriendo en otro contenedor, por ej:

    ```
    links:
    - db:mydatabase
    ```