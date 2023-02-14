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

priv01-platform-ns1-dev-aw-use1-cca-01.dev.exm-platform.com
a6ebc164d4dcc4781a328356064bb007-9b25b9d3b6247fc7.elb.us-east-1.amazonaws.com


https://priv01-platform-ns1-dev-aw-use1-cca-01.dev.exm-platform.com/cdkweb/login

https://priv01-ns1-dev-aw-use1-cca-01.dev.exm-platform.com/converse/health

https://cdk-onboarding.dev.exm-platform.com/cdkweb/login

istio system - NS - Authorizationpolicy

pf -ns1 - vs

URL name - pf-ns1…

kubectl port-forward service/cdk-web 3000:3000

pf-ns1 — running pod

kubectl edit gw dev01-private-gw -n istio-system

```