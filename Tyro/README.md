## Solution for Tyro

1\. add user drogo with the cert and key pair

```
$ kubectl config set-credentials drogo --client-key=/root/drogo.key --client-certificate=/root/drogo.crt
User "drogo" set.
```

2\. Create the `developer` context with user = drogo and cluster = kuberentes

```
$ kubectl config set-context developer --user=drogo --cluster=kubernetes
Context "developer" created.
```

3\. Create the developer-role which has access to all actions in svc,pvc and pods in the development namespace:

```
$ kubectl create role developer-role --verb="*" --resource=svc,pvc,pods -n development
role.rbac.authorization.k8s.io/developer-role created
```

4\. Create the rolebinding for user drogo in the development namespace:

```
$ kubectl create rolebinding developer-rolebinding --role=developer-role --user=drogo -n development
rolebinding.rbac.authorization.k8s.io/developer-rolebinding created
```

5\. Set context 'developer' with user = 'drogo' and cluster = 'kubernetes' as the current context.

```
$ kubectl config use-context developer
Switched to context "developer".
```

6\. Create the jekyll pvc

`kubectl create -f jekyll-pvc.yaml `

7\. Create the jekyll pod

`kubectl create -f jekyll.yaml`

8\. Expose the jekyll pod as a service

`kubectl create -f jekyll-svc.yaml`