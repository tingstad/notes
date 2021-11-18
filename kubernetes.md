# Kubernetes

Assuming namespace `hello`, here are some common commands:

```
kubectl --context prod -n hello get all
kubectl --context prod -n hello get pods
kubectl --context prod -n hello get events --sort-by='.metadata.creationTimestamp'
kubectl --context prod -n hello get deployment/hello -o yaml
kubectl --context prod -n hello get configmap -o json

kubectl --context prod -n hello describe deployment
kubectl --context prod -n hello describe pod [POD]
kubectl --context prod -n hello logs POD [[-c ]CONTAINER]

kubectl --context prod -n hello rollout status deployment/hello
kubectl --context prod -n hello rollout restart deployment/hello
kubectl --context prod -n hello rollout undo deployment/hello

kubectl --context prod -n hello scale --replicas=3 deployment/hello

kubectl --context prod -n hello delete pod POD

kubectl --context prod -n hello exec --stdin --tty POD -- sh

kubectl --context prod -n hello edit configmaps
kubectl --context prod -n hello edit deployment/hello -o yaml

EDITOR="./mod.jq.sh" kubectl --context prod -n hello edit deployment/hello -o json
```

Flux
```
fluxctl release --context prod --timeout 2m0s --workload hello:deployment/hello --update-image=repo/img:tag
```

More: https://kubernetes.io/docs/reference/kubectl/cheatsheet/

