# GitOps demo

### Create a k3d cluster or use existing one

```shell
k3d cluster create gitops-demo --agents "1" --agents-memory "4096m"
```

### Install Argo CD

Install Argo CD:

```shell
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

We suggest that you use our customization to bring back the app of apps sync wave Argo CD feature that was removed in 1.8. You can do this using that command

```shell
kubectl apply -k https://github.com/moabukar/gitops-demo/argocd/manifest
```

### Access Argo CD

Wait for the pod to run: it should go fast, but give it a couple of seconds or minutes depending on your computer. You can check the status with

```shell
kubectl get pods -n argocd
```

Once everything is running, make Argo CD UI accessible locally.

```shell
kubectl -n argocd port-forward svc/argocd-server 8086:443
```

Get the `admin` password:

```shell
# Using kubectl
kubectl get secret argocd-initial-admin-secret -n argocd -o jsonpath={".data.password"} | base64 -d

# Using argocd (Argo CD CLI)
argocd admin initial-password -n argocd
```

Now you can open Argo CD UI. You browser will complain about HTTPS, proceed anyway.

```shell
open https://localhost:8080
```

You can also log into `argocd` with the CLI

```shell
argocd login localhost:8080 --username admin --insecure
```

### Deploy the App of Apps with Sync Waves

You can either deploy the registry using `kubectl` using

```shell
kubectl apply -f https://raw.githubusercontent.com/moabukar/gitops-demo/main/argocd/sync-waves/registry/registry.yaml
```

### Deploy apps reg (optional)

```shell
kubectl apply -f https://raw.githubusercontent.com/moabukar/gitops-demo/main/apps/registry.yaml
```


You can also use `argocd` if you prefer

```shell
argocd app create -f https://raw.githubusercontent.com/moabukar/gitops-demo/main/argocd/sync-waves/registry/registry.yaml
```
