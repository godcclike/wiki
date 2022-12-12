# **Helm Commands**


**To add the helm repo your computer**

    helm repo add <label> <link-to-repo>
    helm repo add my-repo https://charts.bitnami.com/bitnami
    

**To install the helm chart to your computer**

    helm install <label> <package repo>
    helm install mysql my-repo/mysql