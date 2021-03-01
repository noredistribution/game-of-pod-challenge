## Solution for Drupal

## Steps

1. Create the mysql pv

`kubectl create -f drupal-mysql-pv.yaml`

2. Create the mysql pvc

`kubectl create -f drupal-mysql-pvc.yaml`

3. Create the mysql secret

```
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Secret
metadata:
  name: drupal-mysql-secret
type: Opaque
data:
  MYSQL_USER: $(echo -n "root" | base64 -w0)
  MYSQL_ROOT_PASSWORD: $(echo -n "root_password" | base64 -w0)
  MYSQL_DATABASE: $(echo -n "drupal-database" | base64 -w0)
EOF
```

OR

`kubectl create secret generic drupal-mysql-secret --from-literal=MYSQL_ROOT_PASSWORD=root_password --from-literal=MYSQL_DATABASE=drupal-database --from-literal=MYSQL_USER=root`

4. Create the drupal-mysql deployment

`kubectl create -f drupal-mysql.yaml`

5. Expose drupal-mysql deployment as a service

`kubectl create -f drupal-mysql-service.yaml`



6. Create the drupal pv

`kubectl create -f drupal-pv.yaml`

7. Create the drupal pvc

`kubectl create -f drupal-pvc.yaml`

8. Create drupal deployment

`kubectl create -f drupal.yaml`

9. Expose the drupal deployment as a service

`kubectl create -f drupal-service.yaml`
