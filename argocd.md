# Argo CD commands

## To access ArgoCD in browser
```Shell
kubectl port-forward svc/argocd-server -n argocd 8080:443
```

## To login using web browser
```Shell
localhost:8080
```

## To check accounts created in argocd
```Shell
argocd account list
```

## To change password
```Shell
argocd account update-password
```

## To check argocd list of clusters
```Shell
argocd cluster list
```

## Deploying and deleting argocd projects using CLI
```Shell
argocd app list
argocd app get <app name>
argocd app history <app name>
argocd app delete <app name>

argocd app create <app name> \
--project default \
--repo https://github.com/<repo name> \
--path "./<path to the code>" \
--dest-namespace default \
--dest-server https://kubernetes.default.svc

argocd app sync <app name>
```

# argo rollouts commands
| Syntax | Description |
| ----------- | ----------- |
| `kubectl argo rollouts list rollouts` | list all possible version of rollouts |
| `kubectl argo rollouts status simple-rollout` | just to show the status of the rollouts |
| `kubectl argo rollouts get rollout simple-rollout` | get the status on the rollout of the deployment name |
| `kubectl argo rollouts get rollout simple-rollout --watch` | get the live status on the rollout of the deployment name |