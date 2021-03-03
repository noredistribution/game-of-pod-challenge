## Solution for Iron gallery

`kubectl create -f iron-db.yaml`

`kubectl expose deployment iron-db --name=iron-db-service --type=ClusterIP --port=3306 --target-port=3306`

`kubectl create -f netpol-iron-gallery.yaml`

`kubectl create -f iron-gallery-service.yaml`

`kubectl create -f iron-gallery-ingress.yaml`
