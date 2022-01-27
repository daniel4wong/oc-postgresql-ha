# Install dpage/pgadmin4 in Podman


## 1. Install dpage/pgadmin4

### Install dpage/pgadmin4
```
podman run --name pgadmin4 -p 8080:80 \
-e PGADMIN_DEFAULT_EMAIL="username@example.com" \
-e PGADMIN_DEFAULT_PASSWORD="password" \
-d docker.io/dpage/pgadmin4
```


## 2. After install

### Allow 8080 for firewalld
```
sudo firewall-cmd --zone=public --add-port=8080/tcp --permanent
sudo firewall-cmd --reload
```


## Reference
- https://www.pgadmin.org/docs/pgadmin4/latest/container_deployment.html