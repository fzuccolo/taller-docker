# Ejercicio 9

1. Seteo correcto del entorno:

        $ cd ejercicio09
        $ eval $(minikube docker-env)

2. Iniciar deployment:

        $ minikube kubectl -- apply -f deployment.yml

3. Verificar estado de las replicas:

        $ minikube kubectl -- get all

        NAME                                READY   STATUS    RESTARTS   AGE
        pod/password-api-777c9b99f9-8kdms   1/1     Running   0          64s
        pod/password-api-777c9b99f9-fj5qc   1/1     Running   0          64s
        pod/password-api-777c9b99f9-mbm2z   1/1     Running   0          64s

        NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
        service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   5d12h

        NAME                           READY   UP-TO-DATE   AVAILABLE   AGE
        deployment.apps/password-api   3/3     3            3           64s

        NAME                                      DESIRED   CURRENT   READY   AGE
        replicaset.apps/password-api-777c9b99f9   3         3         3       64s

4. Verificar funcionamiento de los pods:

        $ minikube kubectl -- exec -it password-api-777c9b99f9-8kdms -- bash
        root@password-api-777c9b99f9-8kdms:/usr/src/app# curl localhost:3000/password
        {"password":"!35..."}


5. Delete deployment:

        $ minikube kubectl -- delete deployment password-api

        $ minikube kubectl -- get all
        NAME                 TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
        service/kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP   5d12h

