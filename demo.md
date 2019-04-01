**Fetch credentials for your cluster**

gcloud container clusters get-credentials *NAME* [--internal-ip] [--region = *REGION* | --zone = *ZONE*, -z *ZONE*]  --project *PROJECT*

**Clone the demo repo**

```shell
git clone https://github.com/amiramm/guestbook-demo
```
<br>

**Creating a namespace**

```shell
kubectl get namespaces
```

 - Edit the *namespace.json* file, using your first initail + last name as the namespace name and label

```shell
vi guestbook-demo/namespace.json
```

```shell
kubectl create -f guestbook-demo/namespace.json
```

```shell
kubectl get namespaces --show-labels
 ```
 
 ```shell
kubectl config view
```

```shell
kubectl config current-context
```

```shell
kubectl config set-context [namespace] --namespace=[namespace] \
  --cluster=[cluster] \
  --user=[user]
```

```shell
kubectl config use-context [my context]
```
```shell
kubectl config current-context
```
<br>

**Watching for pods being deployed**

```shell
watch -n 5 kubectl get pods
```
 
 **Deploying Redis Backend**
Deploying the master
```shell
kubectl apply -f guestbook-demo/redis-master-deployment.yaml
```
Checking logs
```shell
kubectl logs -f POD-NAME
```
Deploying the master service
```shell
kubectl apply -f guestbook-demo/redis-master-service.yaml
```
Verifying the service
```shell
kubectl get service
```

<br>Deploying the Redis slaves

```shell
kubectl apply -f guestbook-demo/redis-slave-deployment.yaml
```

```shell
kubectl apply -f guestbook-demo/redis-slave-service.yaml
```

```shell
kubectl get service
```

**Deploying Guestbook Frontend**

```shell
kubectl apply -f guestbook-demo/frontend-deployment.yaml
```

Replace NodePort with LoadBalancer
```shell
vi guestbook-demo/frontend-service.yaml
```

```shell
kubectl apply -f guestbook-demo/frontend-service.yaml
```

```shell
kubectl get service
```
Open the EXTERNAL-IP of the frontend

Scale the frontend up to 5 replicas

```shell
kubectl scale deployment frontend --replicas=5
```

Scale the frontend down to w replicas

```shell
kubectl scale deployment frontend --replicas=2
```

**Peering in**
```shell
kubectl exec -it POD-NAME bash
```
```shell
redis-cli
keys *
get messages
exit
exit
```

**Cleaning up **
```shel
kubectl delete deployment redis-slave
kubectl delete deployment redis-master
kubectl delete service -l app=redis
kubectl delete deployment frontend
kubectl delete service -l app=guestbook
```

> Written with [StackEdit](https://stackedit.io/).
