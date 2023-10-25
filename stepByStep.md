# Step by step


## Pods and deployments
Run the test image locally
```shell
docker run --rm -p 2012:2012 --name helloworld -d iundarigun/helloworld
curl http://localhost:2012
curl http://localhost:2012/actuator/health
docker stop helloworld
```

View current pods:
```shell
kubectl get pods
```

Apply pod:
```shell
bat k8sfiles/helloworld.yaml
kubectl apply -f k8sfiles/helloworld.yaml
```

Verify pod (show ready, status, age):
```shell
kubectl get pods
kubectl describe pod helloword
kubectl logs -f --tail=10 helloworld
kubectl port-forward helloworld 2012:2012
```

visit -> http://localhost:2012

Delete a pod:
```shell
kubectl delete pod helloworld
kubectl get pods
```

Apply deployment:
```shell
bat k8sfiles/helloworld-deployment.yaml
kubectl apply -f k8sfiles/helloworld-deployment.yaml 
```

verify (deployment and pod hash):
```shell
kubectl get deployments
kubectl get pods
kubectl describe deployment helloworld
kubectl port-forward helloworld-<deployhash>-<podhash> 2012:2012
```

visit -> http://localhost:2012


Update number of replicas to 3:
```shell
nano k8sfiles/helloworld-deployment.yaml
kubectl apply -f k8sfiles/helloworld-deployment.yaml 
kubectl get pods
kubectl logs -f --tail=10 -l app=helloworld
kubectl delete pod helloworld-<deployhash>-<podhash>
kubectl get pods
```

Delete deployments:
```shell
kubectl delete deployment helloworld
```
Change to 1 or not, and deploy again

## Services

Creating a service

```shell
bat k8sfiles/helloworld-service.yaml
kubectl apply -f k8sfiles/helloworld-service.yaml
kubectl get services
```

Deploy new one to connect:
```shell
bat k8sfiles/connect.yaml
kubectl apply -f k8sfiles/connect.yaml
kubectl port-forward connect-helloworld 2012:2012
```

visit http://localhost:2012/connect

```shell
kubectl logs -f --tail=10 -l app=helloworld
kubectl delete connect-helloworld
```

What happens if we try to deploy a new version:
```shell
bat k8sfiles/helloworld-new-deployment.yaml
kubectl apply -f k8sfiles/helloworld-new-deployment.yaml
kubectl get pods
kubectl port-forward helloworld-<deployhash>-<podhash> 2012:2012
```

visit http://localhost:2012
Explain the errors.

```shell
bat k8sfiles/helloworld-health-deployment.yaml
kubectl apply -f k8sfiles/helloworld-health-deployment.yaml
kubectl get pods
kubectl rollout status deployment/helloworld --timeout=15m
```

## Namespace
```shell
bat k8sfiles/namespace.yaml 
kubectl apply -f k8sfiles/namespace.yaml
kubectl get namespaces
kubectl apply -f k8sfiles/helloworld.yaml -n local-test
kubectl get pods
kubectl get pods -n local-test
```

## More and more and more

Config kube file
k9s
