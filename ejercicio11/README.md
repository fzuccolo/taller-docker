# Ejercicio 11

1. Crear un namespace para resolver el ejercico (opcional):

        $ cd ejercicio11

        $ kubectl create namespace ej11
        namespace/ej11 created

        $ kubectl config set-context --current --namespace=ej11
        Context "minikube" modified.

        $ kubectl get all
        No resources found in ej11 namespace.

2. Definir los descriptores de los objetos:

    * `configmap1.yaml`
    * `configmap2.yaml`
    * `secret.yaml`
    * `service.yaml`
    * `deployment.yaml`

3. Aplicar descriptores:

        $ kubectl apply -f configmap1.yaml
        configmap/pingapp-config-env created

        $ kubectl apply -f configmap2.yaml
        configmap/pingapp-config-files created

        $ kubectl apply -f secret.yaml
        secret/pingapp-secret-files configured

        $ kubectl apply -f service.yaml
        service/pingapp created

        $ kubectl apply -f deployment.yaml
        deployment.apps/pingapp created

   Alternativamente, se puede pasar el directorio como parámetro:

        $ kubectl apply -f ./
        configmap/pingapp-config-env unchanged
        configmap/pingapp-config-files unchanged
        deployment.apps/pingapp unchanged
        secret/pingapp-secret-files configured
        service/pingapp unchanged

4. Verificar deploy:

        $ kubectl get all

        NAME                           READY   STATUS    RESTARTS   AGE
        pod/pingapp-694b4b98b6-v6g8n   1/1     Running   0          12m
        pod/pingapp-694b4b98b6-vqbfq   1/1     Running   0          12m
        pod/pingapp-694b4b98b6-xgn5f   1/1     Running   0          12m

        NAME              TYPE       CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
        service/pingapp   NodePort   10.102.220.138   <none>        8080:32019/TCP   12m

        NAME                      READY   UP-TO-DATE   AVAILABLE   AGE
        deployment.apps/pingapp   3/3     3            3           12m

        NAME                                 DESIRED   CURRENT   READY   AGE
        replicaset.apps/pingapp-694b4b98b6   3         3         3       12m

5. Verificar configuración desde uno de los pods:

        $ kubectl exec -it pod/pingapp-694b4b98b6-v6g8n -- bash

        appuser@pingapp-694b4b98b6-v6g8n:/apps/pingapp$ env | grep LOCATION
        CONFIG_LOCATION=/config/config.json
        SECRETS_LOCATION=/secrets/secret.json

        appuser@pingapp-694b4b98b6-v6g8n:/apps/pingapp$ cat /config/config.json
        {"key1": "value1"}

        appuser@pingapp-694b4b98b6-v6g8n:/apps/pingapp$ cat /secrets/secret.json
        {"mypass":"password!"}

6. Verificar comportamiento del servicio desde el host:

   Obtener IP del servicio:

        $ minikube service pingapp --url -n ej11
        http://192.168.49.2:32019

   Notar el cambio de host que atiende cada solicitud:

        $ curl 192.168.49.2:32019
        {"version":"2.1.0","ping":"2022-07-03T20:55:00+00:00","config_location":"/config/config.json","secrets_location":"/secrets/secret.json","host":"pingapp-694b4b98b6-v6g8n"}

        $ curl 192.168.49.2:32019
        {"version":"2.1.0","ping":"2022-07-03T20:55:02+00:00","config_location":"/config/config.json","secrets_location":"/secrets/secret.json","host":"pingapp-694b4b98b6-vqbfq"}

        $ curl 192.168.49.2:32019
        {"version":"2.1.0","ping":"2022-07-03T20:55:04+00:00","config_location":"/config/config.json","secrets_location":"/secrets/secret.json","host":"pingapp-694b4b98b6-v6g8n"}

        $ curl 192.168.49.2:32019
        {"version":"2.1.0","ping":"2022-07-03T20:55:07+00:00","config_location":"/config/config.json","secrets_location":"/secrets/secret.json","host":"pingapp-694b4b98b6-xgn5f"}

7. Verificar que el servicio retorna los datos definidos:

        $ curl 192.168.49.2:32019/config
        {"key1": "value1"}

        $ curl 192.168.49.2:32019/secrets
        {"mypass":"password!"}


#### Referencias

* https://kubernetes.io/es/docs/concepts/configuration/secret/

