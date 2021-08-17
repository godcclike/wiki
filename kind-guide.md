# Kind Guide

## To see different kubernetes context
```Shell
kubectl config get-contexts
```

## Change context between aws and kind
```Shell
kubectl config use-context <context name>     (from first command)
```

## To see the current context
```Shell
kubectl config current-context
```