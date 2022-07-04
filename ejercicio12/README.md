# Ejercicio 12 (incompleto)

1. Definir descriptor para pod de postgres:

   * `stateful-set.yaml`

2. Definir descriptores para entorno de jobvacancy:

   * `configmap.yaml`
   * `secret.yaml`

   El valor de `DATABASE_URL` debe estar en base64:

        $ echo "postgres://postgres:passwoord@postgres-db:5432/postgres" | base64
        cG9zdGdyZXM6Ly9wb3N0Z3JlczpwYXNzd29vcmRAcG9zdGdyZXMtZGI6NTQzMi9wb3N0Z3Jlcwo=

3. Aplicar descriptores:

        $ kubectl apply -f stateful-set.yaml
        $ kubectl apply -f configmap.yaml
        $ kubectl apply -f secret.yaml
        $ kubectl apply -f web_deployment.yaml

4. El pod de `jobvacancy` queda en error:

        $ k get all

        NAME                              READY   STATUS             RESTARTS      AGE
        pod/jobvacancy-79b8645857-7sl7n   0/1     CrashLoopBackOff   4 (20s ago)   116s
        pod/postgres-db-0                 1/1     Running            0             3m7s

        NAME                         READY   UP-TO-DATE   AVAILABLE   AGE
        deployment.apps/jobvacancy   0/1     1            0           116s

        NAME                                    DESIRED   CURRENT   READY   AGE
        replicaset.apps/jobvacancy-79b8645857   1         1         0       116s

        NAME                           READY   AGE
        statefulset.apps/postgres-db   1/1     3m7s


5. Inspección del error:

        $ kubectl logs pod/jobvacancy-79b8645857-7sl7n
        rake aborted!
        URI::InvalidURIError: bad URI(is not URI?): postgres://postgres:passwoord@postgres:5432/postgres


6. (A completar...)


#### References

https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/

