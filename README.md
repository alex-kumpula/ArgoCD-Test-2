# ArgoCD Cluster Boostrapping

This repo serves to act as the source of truth for a GitOps-based ArgoCD-managed Kubernetes cluster.

In order to bootstrap a cluster with this repository, there are two steps: Installing ArgoCD and Adding This Repository.

## Installing ArgoCD

To install ArgoCD into your cluster, do the following:

Run the commands:

```
helm dependency update charts/argo-cd/
```

```
helm install argo-cd charts/argo-cd/ -n argocd --create-namespace
```

Now you can access the Argo-CD webpage by doing the following:

```
kubectl port-forward svc/argo-cd-argocd-server 8080:443 -n argocd
```

This will port-forward the ArgoCD dashboard to `localhost:8080`.

To login to the dashboard, use the username "admin", and the password retrieved by this command:

```
kubectl get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" -n argocd | base64 -d
```

Now ArgoCD is installed in your cluster.

## Adding This Repository

`helm template charts/root-app/ | kubectl apply -f -`

There you go! Now you have a Argo-CD git-ops cluster bootstrapped and ready to go!
