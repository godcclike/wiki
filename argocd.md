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