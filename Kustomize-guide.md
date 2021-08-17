## **To check content**

 ```shell
kustomize build ms-pats-order-management/overlays/global-env | less

kustomize build ms-pats-order-management/overlays/5-env | less

kustomize build ms-pats-users/overlays/qa-env | less | grep namespace
```

## **To apply changes manually**
### **NOTE: ArgoCD will revert changes if you do the below**

 ```shell
kustomize build ms-pats-users/overlays/global-env | kaf -

kustomize build overlays/qa-env | kaf - 
```

