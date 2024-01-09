## Routing in K8

#### Service.Type=NodePort
Create deployment and expost on port 8080
```
kubectl create deployment hello-minikube --image=kicbase/echo-server:1.0
kubectl expose deployment hello-minikube --type=NodePort --port=8080
```

Check deployment
```
kubectl get services hello-minikube
```

Access service
```
minikube service hello-minikube
```

Forward the port
```
kubectl port-forward service/hello-minikube 7080:8080
```

Application available at localhost:7080

#### Service.Type=LoadBalancer
```
kubectl create deployment balanced --image=kicbase/echo-server:1.0
kubectl expose deployment balanced --type=LoadBalancer --port=8080
```

Start the tunnel to create a routable IP for the ‘balanced’ deployment
```
minikube tunnel
```
*what's a "tunnel"?

Find the <external-ip>
```
kubectl get services balanced
```

#### Ingress
Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the cluster. Traffic routing is controlled by rules defined on the Ingress resource.

Enable ingress addon
```
minikube addons enable ingress
```

Yaml - create 2 pods, 2 services  and some ingress rules to expose these 2 services at 2 different paths
```
kubectl apply -f https://storage.googleapis.com/minikube-site-examples/ingress-example.yaml
```

```
kubectl get ingress
```

Need tunnelling for Docker Desktop to allow routable IP 127.0.0.1
```
minikube tunnel
```
