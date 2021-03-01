## Solution for Pento

1\. To fix kube-apiserver, check the logs

```
docker ps -a  | grep apiserver
docker logs -f <containerID>
```

At the end it will throw something like this:

`error: unable to load client CA file: unable to load client CA file: open /etc/kubernetes/pki/ca-authority.crt: no such file or directory`

The real name of the file is ca.crt, so it has to be modified in `/etc/kubernetes/manifests/kube-apiserver.yaml`

2\. The kube config has the wrong port for the server, it should be `6443`, simply editing `/root/.kube/config` will do the trick.

3\. To fix the `ImagePullError` issue for coredns we can simply edit the deployment and correct he image name
`kubectl -n kube-system edit deployments coredns`


4\. How to make node01 schedulable again?

```
controlplane $ kubectl get nodes
NAME           STATUS                     ROLES    AGE   VERSION
controlplane   Ready                      master   40m   v1.14.0
node01         Ready,SchedulingDisabled   <none>   39m   v1.14.0
controlplane $
controlplane $ kubectl uncordon node01
node/node01 uncordoned
controlplane $ kubectl get nodes
NAME           STATUS   ROLES    AGE   VERSION
controlplane   Ready    master   40m   v1.14.0
node01         Ready    <none>   40m   v1.14.0
```

5\. Create the data pv

`$ kubectl create -f data-pv.yaml`


6\. Create data pvc

```
controlplane $ kubectl create -f data-pvc.yaml
persistentvolumeclaim/data-pvc created
controlplane $ kubectl get pv
NAME      CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM              STORAGECLASS   REASON   AGE
data-pv   1Gi        RWX            Retain           Bound    default/data-pvc                           2m42s
```

7\. Create the filesserver

`$ kubectl create -f gop-file-server.yaml`

8\. Create the fs service

`kubectl create -f gop-fs-service.yaml`



