# Istio commands

## Things to edit when changing istio hosts
    1. virtual service
    2. gateway
    3. authorization policy

## To get a list of virtual services and edit it
```Shell
kubectl get vs -n <namespace>

kubectl describe vs <vs-name> -n <namespace>

kubectl edit vs <vs-name> -n <namespace>
```

## To see the Authorization Policy and Edit it
```Shell
kubectl get authorizationpolicy -n istio-system

kubectl get authorizationpolicy dev01-policy -o yaml -n istio-system

kubectl describe authorizationpolicy dev01-policy -n istio-system

kubectl edit authorizationpolicy dev01-policy -n istio-system
```

## To see the istio gateway and edit it
```Shell
kubectl get gw -n istio-system

kubectl describe gw <gw-name> -n istio-system

kubectl edit gw <gw-name> -n istio-system
```

## Troubleshooting
```Shell
kubectl port-forward service/cdk-web 3000:3000

```