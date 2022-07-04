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