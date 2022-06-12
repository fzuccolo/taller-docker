# Ejercicio 2

1. Descargar imagen:

        docker pull nicopaez/pingapp:3.0.0

2. Crear [repositorio en dockerhub](https://hub.docker.com/r/fzuccolo/pingapp):

        fzuccolo/pingapp

3. Taggear imagen:

        docker image tag nicopaez/pingapp:3.0.0 fzuccolo/pingapp:3.0.0

4. CLI login con credenciales del punto 2:

        docker login

5. Publicar imagen:

        docker push fzuccolo/pingapp:3.0.0

6. Descargar imagen pública:

        docker pull fzuccolo/pingapp:3.0.0
