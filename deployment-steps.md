# Deployment steps for kubernetes

## ms-pats-order-management
```Shell
docker build -t ms-pats-order-management .
docker tag ms-pats-order-management:latest 135793799950.dkr.ecr.ap-northeast-1.amazonaws.com/ms-pats-order-management:latest
docker push 135793799950.dkr.ecr.ap-northeast-1.amazonaws.com/ms-pats-order-management:latest
kubectl apply -f config/sg/sit/deployment-config.yml
kubectl rollout restart deploy ms-pats-order-management
```

## ms-pats-users
```Shell
docker build -t ms-pats-users .
docker tag ms-pats-users:latest 135793799950.dkr.ecr.ap-northeast-1.amazonaws.com/ms-pats-users:latest
docker push 135793799950.dkr.ecr.ap-northeast-1.amazonaws.com/ms-pats-users:latest
kubectl apply -f config/sg/sit/deployment-config.yml
kubectl rollout restart deploy ms-pats-users
```

## ms-pats-simulator
```Shell
docker build -t ms-pats-simulator .
docker tag ms-pats-simulator:latest 135793799950.dkr.ecr.ap-northeast-1.amazonaws.com/ms-pats-simulator:latest
docker push 135793799950.dkr.ecr.ap-northeast-1.amazonaws.com/ms-pats-simulator:latest
kubectl apply -f config/sg/sit/deployment-config.yml
kubectl rollout restart deploy ms-pats-simulator
```

## ECR
```Shell
aws ecr get-login-password --region ap-northeast-1 | docker login --username AWS --password-stdin 135793799950.dkr.ecr.ap-northeast-1.amazonaws.com
```

## Kubernetes Dasboard
```Shell
kubectl proxy --port=8080 --address=0.0.0.0 --disable-filter=true &

aws eks get-token --cluster-name eks-cluster-ocbc | jq -r '.status.token'

http://localhost:8080/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login
```

## EKS login (only first time)
```Shell
aws eks --region ap-northeast-1 update-kubeconfig --name eks-cluster-ocbc
```