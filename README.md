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