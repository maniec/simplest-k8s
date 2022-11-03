# simplest-k8s

From Docker official [Orchestration](https://docs.docker.com/get-started/orchestration/) documentations

## Enable Kubernetes
```yaml
# pod.yaml
apiVersion: v1
kind: Pod
metadata:
name: demo
spec:
containers:
- name: testpod
  image: alpine:latest
  command: ["ping", "8.8.8.8"]
```
```shell
kubectl apply -f pod.yaml
kubectl get pods
kubectl logs demo
kubectl delete -f pod.yaml
```


## Enable Docker Swarm
```shell
docker swarm init
docker service create --name demo alpine:latest ping 8.8.8.8
docker service ps demo
docker service logs demo
docker service rm demo
```

## Deploy to Kubernetes
```yaml
# bb.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: bb-demo
  namespace: default
spec:
  replicas: 1
  selector:
    matchLabels:
      bb: web
  template:
    metadata:
      labels:
        bb: web
    spec:
      containers:
      - name: bb-site
        image: getting-started
        imagePullPolicy: Never
---
apiVersion: v1
kind: Service
metadata:
  name: bb-entrypoint
  namespace: default
spec:
  type: NodePort
  selector:
    bb: web
  ports:
  - port: 3000
    targetPort: 3000
    nodePort: 30001
```
```shell
kubectl apply -f bb.yaml
kubectl get deployments
kubectl get services
```
Access localhost:30001
```shell
kubectl delete -f bb.yaml
```