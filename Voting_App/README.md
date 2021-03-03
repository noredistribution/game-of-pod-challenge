## Solution for Voting App

`kubectl create -f vote-deployment.yaml`
`kubectl create -f vote-service.yaml`
`kubectl create -f redis-deployment.yaml`  

Create service for redis-deployment
`$ kubectl -n vote expose deployment redis-deployment --type=ClusterIP --port=6379 --target-port 6379`

`kubectl create -f db-deployment.yaml`

Create service for db-deployment
`kubectl -n vote expose deployment db-deployment --name=db --type=ClusterIP --port=5432 --target-port=5432`

`kubectl create -f result-deployment.yaml`
`kubectl create -f result-service.yaml `
`kubectl create -f worker.yaml`
