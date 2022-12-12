# **Helm Commands**


**To add the helm repo to your kube cluster**

    helm repo add <label> <link-to-repo>
    helm repo add my-repo https://charts.bitnami.com/bitnami
    
**To install the helm chart to your kube cluster**

    helm install <chart-name> <package repo>
    helm install mysql my-repo/mysql
    
**To list all the helm charts/packages that is installed in the kube cluster**

    helm list
    helm ls

**To uninstall a helm chart**

    helm uninstall <chart-name>

