# Install dpage/pgadmin4 in OpenShift


## 1. Before installation

### Create serviceaccount to resolve permission for dpage/pgadmin4
- `oc create sa pgadmin`
- `oc adm policy add-scc-to-user anyuid -z pgadmin`


## 2. Install dpage/pgadmin4

### Install dpage/pgadmin4
```
oc new-app docker.io/dpage/pgadmin4 --name pgadmin4 \
-e PGADMIN_DEFAULT_EMAIL="username@example.com" \
-e PGADMIN_DEFAULT_PASSWORD="password"
```


## 3. After install

### Set service account
- `oc set serviceaccount deployment/pgadmin4 pgadmin`

### Create route for pgadmin
- `oc expose service pgadmin4`
- `oc get route`


## 5. Uninstall all resources
- `oc get all -l app=pgadmin4`
- `oc delete all -l app=pgadmin4`


## Reference
- https://www.pgadmin.org/docs/pgadmin4/latest/container_deployment.html