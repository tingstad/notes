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
kubectl --context prod -n hello [--previous] logs POD [[-c ]CONTAINER]

kubectl --context prod -n hello rollout status deployment/hello
kubectl --context prod -n hello rollout restart deployment/hello
kubectl --context prod -n hello rollout undo deployment/hello

kubectl --context prod -n hello scale --replicas=3 deployment/hello

kubectl --context prod -n hello delete pod POD

kubectl --context prod -n hello exec --stdin --tty POD -- sh

kubectl --context prod -n hello edit configmaps
kubectl --context prod -n hello edit deployment/hello -o yaml

EDITOR="./mod.jq.sh" kubectl --context prod -n hello edit deployment/hello -o json

kubectl debug ... #if EphemeralContainers is enabled in cluster, otherwise:

kubectl --context prod -n hello run mypod -it --rm --image postgres --env PGPASSWORD="$pw" \
    --command -- psql -h host -U user -d dbname  #mypod can be any name
```

More: https://kubernetes.io/docs/reference/kubectl/cheatsheet/


## Kustomize

```
kustomize build kustomize/env/path/ | kubectl apply -f -
```

Inline [6902](https://datatracker.ietf.org/doc/html/rfc6902) operation when strategic merge doesn't cut it:
```
patchesJson6902:
- target:
    group: apps
    version: v1
    kind: Deployment
    name: foo
    namespace: foo
  patch: |-
    - op: replace
      path: /spec/template/spec/containers/0/ports/0/containerPort
      value: 8080
```

## Flux
```
fluxctl release --context prod --timeout 2m0s --workload hello:deployment/hello --update-image=repo/img:tag
```

