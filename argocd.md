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

argocd app create cca_api_dev --repo git@ghe.exm-platform.com:Telepathy-Labs/cca_api.git --path 10_deploy --dest-namespace argocd-demo-deployment --dest-server https://66F5D9B388DA3C87C24008F2D8B33F25.gr7.us-east-1.eks.amazonaws.com --values values.yaml --project ns1-dev-aw-use1-cca-01

argocd app create reporting-service --repo git@ghe.exm-platform.com:Telepathy-Labs/reporting_service.git --path 10_deploy --dest-namespace argocd-demo-deployment --dest-server https://66F5D9B388DA3C87C24008F2D8B33F25.gr7.us-east-1.eks.amazonaws.com --values values.yaml --project ns1-dev-aw-use1-cca-01
```

# argo rollouts commands
| Syntax | Description |
| ----------- | ----------- |
| `kubectl argo rollouts list rollouts` | list all possible version of rollouts |
| `kubectl argo rollouts status simple-rollout` | just to show the status of the rollouts |
| `kubectl argo rollouts get rollout simple-rollout` | get the status on the rollout of the deployment name |
| `kubectl argo rollouts get rollout simple-rollout --watch` | get the live status on the rollout of the deployment name |