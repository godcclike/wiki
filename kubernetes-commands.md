# Kubetnetes Commands

## Kubernetes Deployments commands
```Shell
kubectl get deployments
kubectl delete deployment <deployment name>
kubectl apply -f <path-to-the-config.yml>
kubectl rollout restart deploy <deployment name>
```

## Kubernetes Pods commands
```Shell
kubectl get pods
kubectl delete pod <pod name>
kubectl describe <pod name>
```

## Kubernetes Serivce commands
```Shell
kubectl get svc
kubectl delete svc <svc name>
```

## Kubernetes Namespace commands
```Shell
kubectl get namespaces
kubectl config view
kubectl create namespace <new namespace>
kubectl delete namespace <namespace>
kubectl config set-context --current --namespace=<namespace>
```

## To change context
```Shell
kubectl logs -f <pod name>
kubectl get secret
kubectl config get-contexts
kubectl config current-context
kubectl config use-context <context name>
kubectl config use-context arn:aws:eks:ap-northeast-1:135793799950:cluster/eks-cluster-ocbc
```

## Kube dashboard
```Shell
kubectl proxy --port=8080 --address=0.0.0.0 --disable-filter=true &

aws eks get-token --cluster-name eks-cluster-ocbc | jq -r '.status.token'

http://localhost:8080/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login
```

## Download a file from a pod
```Shell
kubectl cp shanghai-sit/ms-pats-simulator-5f49786d45-2tzkt:/app-build/<source file name> <target file name>
kubectl cp shanghai-sit/ms-pats-simulator-5f49786d45-2tzkt:/app-build/heap.hprof.simulator.tar.gz   heap.hprof.simulator.tar.gz
```

## Minikube
| Syntax | Description |
| ----------- | ----------- |
| `kubectl create deployment <deployment name> --image=<image name>` | kubectl create deployment nginx-deployment --image=nginx |
| `kubectl get replicaset` |
| `kubectl exec -it <pod name> bash` |
| `kubectl get pod -o wide`  | to see more details about all pods |
