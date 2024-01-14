minikube for local kubernetes practise

```
minikube start
```


```
get resources
alvinvoo@alvin-nitro:~/my_projects/ckad$ kubectl get po -A
NAMESPACE     NAME                               READY   STATUS    RESTARTS      AGE                  kube-system   coredns-5dd5756b68-6kqxf           1/1     Running   0             26s                  kube-system   etcd-minikube                      1/1     Running   0             38s
kube-system   kube-apiserver-minikube            1/1     Running   0             38s
kube-system   kube-controller-manager-minikube   1/1     Running   0             38s
kube-system   kube-proxy-wh694                   1/1     Running   0             27s
kube-system   kube-scheduler-minikube            1/1     Running   0             38s
kube-system   storage-provisioner                1/1     Running   1 (16s ago)   37s
```

To create and expose deployment (which will ultimately create the pods)
```
kubectl create deployment hello-minikube --image=kicbase/echo-server:1.0
kubectl expose deployment hello-minikube --type=NodePort --port=8080
```


To create a pod directly
```
kubectl run nginx --image nginx
```

To create as dry-run and output into yaml format
```
alvinvoo@alvin-nitro:~/my_projects/ckad$ kubectl run redis --image=redis123 --dry-run=client -o yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: redis
  name: redis
spec:
  containers:
  - image: redis123
    name: redis
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
```


To check the pods
```
kubectl get pods

kubectl get pods --selector app=App1
```

Check pods with more info
```
alvinvoo@alvin-nitro:~/my_projects/ckad$ kubectl get pods -o wide
NAME      READY   STATUS    RESTARTS      AGE   IP            NODE       NOMINATED NODE   READINESS GATES
bar-app   1/1     Running   1 (68s ago)   15h   10.244.0.22   minikube   <none>           <none>
foo-app   1/1     Running   1 (68s ago)   15h   10.244.0.19   minikube   <none>           <none>
```

To check the services
```
kubectl get services
```

To access the service
(it will do the auto forwarding of port and open the browser)
```
minikube service hello-minikube
```

Or forward manually
```
kubectl port-forward service/hello-minikube 7080:8080
```

```
kubectl describe pod <pod-name>
```

To scale the Replica Set
Change file and `replace` it
```
kubectl replace -f replicaset-definition.yml
```

Scale with file input (but not changing the file initial replicas value)
```
kubectl scale --replicas=6 -f replicaset-definition.yml
```

Scale with `type` and `name`
```
kubectl scale --replicas=6 replicaset myapp-replicaset
```

[Replica set commands](./replica-set-commands.png)

```
kubectl explain replicaset
```

```
all same
alvinvoo@alvin-nitro:~/my_projects/ckad$ kubectl get rs
NAME               DESIRED   CURRENT   READY   AGE
myapp-replicaset   3         3         3       24h
alvinvoo@alvin-nitro:~/my_projects/ckad$ kubectl get replicasets
NAME               DESIRED   CURRENT   READY   AGE
myapp-replicaset   3         3         3       24h
alvinvoo@alvin-nitro:~/my_projects/ckad$ kubectl get replicaset
NAME               DESIRED   CURRENT   READY   AGE
myapp-replicaset   3         3         3       24h
```

Command to show everything created by k8
```
kubectl get all

# --no-headers option is good, then can count properly
kubectl get all --selector env=prod --no-headers | wc -l

```

### Namespaces
Target namespace
```
kubectl get pods --namespace=kube-system
kubectl get pods -n=kube-system
```

List for all namespaces
```
kubectl get pods --all-namespaces
kubectl get pods -A
```

### Services
```
kubectl get services
kubectl get svc
```

### Imperative style command to generate definition file and save time
```
# Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)
kubectl run nginx --image=nginx --dry-run=client -o yaml

# Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)
kubectl create deployment --image=nginx nginx --dry-run -o yaml

# Generate Deployment with 4 Replicas
kubectl create deployment nginx --image=nginx --replicas=4

# Scaling
kubectl scale deployment nginx --replicas=4
```

```
# Create a Service named redis-service of type ClusterIP to expose pod redis on port 6379
# will use pod's labels as selectors
kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml

# aaassume selectors as app=redis
kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml

```

Creating a pod and expose it
```
kubectl create pod httpd --image=httpd:alpine
kubectl expose pod httpd --name=httpd --port=80 --target-port=80

# to watch it getting ready and running
kubectl create pod httpd --image=httpd:alpine --watch

OR

#use the expose option, it will create a service with same name
kubectl create pod httpd --image=httpd:alpine --expose=true
```

Replacing directly
(delete + create)
```
kubectl replace --force -f /tmp/<filename>.yml
```

ConfigMaps
```
kubectl get configmaps

kubectl describe configmaps
```

```
# use apply on declarative style (with definition file)
# apply will create the resource if not yet exist
kubectl apply -f <filename>
```

## Secrets
```
kubectl get secrets

kubectl describe secrets
```

Docker adding and dropping capabilities
```
docker run --cap-add MAC_ADMIN ubuntu
docker run --cap-drop MAC_ADMIN ubuntu

# allow full capabilities
docker run --privileged ubuntu
```

## Service Account
```
kubectl create serviceaccount dashboard-sa

kubectl get serviceaccount

kubectl describe serviceaccount dashboard-sa

# create secret token
kubectl create token dashboard-sa
```


```
kubectl get nodes
```

```
# to check for logs in /log/app.log
kubectl logs app 

# alternatively can exec and cat the file
kubectl exec -it app sh

kubectl exec -it app -- cat /log/app.log 
```

```
# to get and edit ALL pods
 
kubectl get pod -o yaml > webapp.yml
kubectl delete pod --all
vim webapp.yaml # this will have all the pods definitions
```

```
# use -A as --all-namespaces
k get deploy -A
```

```
k get networkpolicies
k get netpol
```

```
to list all k8s objects

kubectl api-resources
```

## Kube config
```
kubectl config view

kubectl config use-context <some new context>

kubectl config -h
```

## HELM commands
```
-- search default artifact hub io 
helm search hub wordpress

-- add repo
helm repo add bitnami <url..>

-- search repo
helm search repo wordpress

-- list repos
helm repo list

-- install
helm install <release-name> <chart-name>

-- list releases
helm list

helm uninstall <release id>

-- download and untar the helm charts
helm pull --untar bitnami/wordpress

-- after making changes, then install
helm install release-4 ./wordpress
```
