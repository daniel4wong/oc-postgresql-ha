# Install bitnami/postgresql-ha in OpenShift


## 1. Before installation

### Update values.yaml
- values in pgpool.sidecars.env
  - PGADMIN_DEFAULT_EMAIL: username@example.com
  - PGADMIN_DEFAULT_PASSWORD: password

### Create serviceaccount to resolve permission for dpage/pgadmin4
- `oc create sa postgresql`
- `oc adm policy add-scc-to-user anyuid -z postgresql`


## 2. Install bitnami/postgresql-ha

### Add bitnami to repo
- `helm repo add bitnami https://charts.bitnami.com/bitnami`

### Install bitnami/postgresql-ha
- `helm install postgresql-ha bitnami/postgresql-ha -f values.yaml`

### Uninstall bitnami/postgresql-ha and pvc
- `helm uninstall postgresql-ha && oc delete pvc` (Information)


## 3. After install

### Set service account
- `oc set serviceaccount deployment/postgresql-ha-pgpool postgresql`

### Create service and route for pgadmin
- `oc apply -f postgresql-ha-pgadmin.yaml`
- `oc expose service postgresql-ha-pgadmin`


## 4. Debug

### Export password
- `export POSTGRES_PASSWORD=$(kubectl get secret --namespace dev-postgresql postgresql-ha-postgresql -o jsonpath="{.data.postgresql-password}" | base64 --decode)`
- `export REPMGR_PASSWORD=$(kubectl get secret --namespace dev-postgresql postgresql-ha-postgresql -o jsonpath="{.data.repmgr-password}" | base64 --decode)`

### Forward the port to local
- `kubectl port-forward --namespace dev-postgresql svc/postgresql-ha-pgpool 5432:5432 &`


## 5. Uninstall all resources
- `helm uninstall postgresql-ha`
- `oc delete pvc -l app.kubernetes.io/name=postgresql-ha`
- `oc delete all -l app.kubernetes.io/name=postgresql-ha` (Optional)
