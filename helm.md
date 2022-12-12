# **Helm Commands**


**To add the helm repo to your kube cluster**

    helm repo add <label> <link-to-repo>
    helm repo add my-repo https://charts.bitnami.com/bitnami

**To list all helm repo that you have added**

    helm repo update
    helm repo list
    
**To install the helm chart to your kube cluster**

    helm install <chart-name> <package repo>
    helm install mysql my-repo/mysql
    
**To list all the helm charts/packages that is installed in the kube cluster**

    helm list
    helm ls

**To download the entire helm repo and untar it (you will get the charts.yml, values.yml to edit)**
    
    helm pull bitnami/mysql --untar=true

**To install using your own downloaded repositories from earlier**

    helm install <chart-name> </repository-path>
    helm install <chart-name> .

**To uninstall a helm chart**

    helm uninstall <chart-name>

**To show the values of the helm chart in the current directory**

    helm show values .

**To dry run helm and output the values into a file (use helm upgrade when changing values because it is already installed)**

    helm upgrade ns2-stg-aw-use1-cca-01 .        --namespace ns2-stg-aw-use1-cca-01      -f ./environments/ns2-stg-aw-use1-cca-01.yml -f release_versions/release_1.10.0.yaml --dry-run --debug > testing.yml