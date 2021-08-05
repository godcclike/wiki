# OMS-PAT Kubernetes Setup
---
## prerequisites
- kubectl
- docker
- vaild SSL certificates
- aws cli
- aws ecr registory

###  Installing AWS Cli.
- **Step 1**:

    ```shell
    $ curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
    $ unzip awscliv2.zip
    $ sudo ./aws/install

    ```

- **Step 2**: configure aws-cli

    ```shell
    $ aws configure
    ```

### Creating Wildcard SSL Certificate.
- **Step 1**:

    Install **Certbot**.
    ```shell
    $ sudo apt install certbot
    ```
- **Step 2**:

    ```shell
    $ sudo certbot certonly --manual --preferred-challenges=dns --email akhil.ph@centelon.com \
      --server https://acme-v02.api.letsencrypt.org/directory \
      --agree-tos --manual-public-ip-logging-ok -d "*.platinumecn.com"

    ```
- **Step 3**:
Create a new **TXT** record in DNS manager with the key displayed from step 2.

    > Example: _acme-challenge.platinumecn.com TXT "uwbELGXAejbDPGOFXcV2oPrNX7rccp8JkrFFxn-Sw8E"

- **Step 4**:
Once the DNS challenge is successful, the certificate will be generated at **/etc/letsencrypt/archive/platinumecn.com**.

### Creating Keystore and Cacert from SSL Cert.
- **Step 1**: 

    Creating **cacert** for **CAS**:
    ```shell
    $ keytool -import -alias cas-qa.platinumecn.com -keystore $JAVA_HOME/jre/lib/security/cacerts \
      -file fullchain1.pem -storepass changeit -noprompt
    ```
    Creating **Keystore** for **CAS**:
    ```shell
    $ openssl pkcs12 -export -in fullchain1.pem -inkey privkey1.pem -out pats.p12 -name cas-qa.platinumecn.com
    $ keytool -importkeystore -destkeystore keystore.jks -srckeystore pats.p12 -srcstoretype pkcs12 -alias cas-qa.platinumecn.com
    $ keytool -importkeystore -srckeystore keystore.jks -destkeystore keystore.jks -deststoretype pkcs12
    ```
- **Step 2**:

    Creating **cacert** for **oms-web**:
    ```shell
    $ keytool -import -alias frontend-qa.platinumecn.com -keystore $JAVA_HOME/jre/lib/security/cacerts \
      -file fullchain1.pem -storepass changeit -noprompt
    ```
    Creating **Keystore** for **oms-web**:
    ```shell
    $ openssl pkcs12 -export -in fullchain1.pem -inkey privkey1.pem -out pats.p12 -name frontend-qa.platinumecn.com
    $ keytool -importkeystore -destkeystore keystore.jks -srckeystore pats.p12 -srcstoretype pkcs12 -alias frontend-qa.platinumecn.com
    $ keytool -importkeystore -srckeystore keystore.jks -destkeystore keystore.jks -deststoretype pkcs12
    ```
### Login into AWS ECR.

```shell
$ aws ecr get-login-password --region ap-northeast-1 | docker login --username AWS --password-stdin 135793799950.dkr.ecr.ap-northeast-1.amazonaws.com
```
### Building Docker images.
> Do this for all micro services.
```shell
$ docker build -t local-image:latest .
```
### Pushing docker images to ECR
> Do this for all micro services.
```shell
$ docker tag local-image:latest 135793799950.dkr.ecr.ap-northeast-1.amazonaws.com/remote-image:v1
$ docker tag local-image:latest 135793799950.dkr.ecr.ap-northeast-1.amazonaws.com/remote-image:latest
$ docker push 135793799950.dkr.ecr.ap-northeast-1.amazonaws.com/remote-image:v1
$ docker push 135793799950.dkr.ecr.ap-northeast-1.amazonaws.com/remote-image:latest
```

### Configure Kubectl.
```shell
$ aws eks --region ap-northeast-1 update-kubeconfig --name eks-cluster-ocbc
$ kubectl get ns
```
### Creating TLS Secret for Ingress TLS
> Rename the fullchain.pem to star.platinumecn.com.crt
```shell
$ kubectl create secret tls oms-ssl --key privkey1.pem --cert star.platinumecn.com.crt
```
### Ingress Controller Sample
> Sample Ingress config for CAS.
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: pats-fx-users-ingress-qa
  namespace: pats-fx-oms-app-global-qa
  labels:
      name: pats-fx-users-ingress-qa
  annotations:
    kubernetes.io/ingress.class: "nginx"
    nginx.ingress.kubernetes.io/ssl-passthrough: "true"   ## This Annotation is required for proper tls termination to cas service,
    nginx.ingress.kubernetes.io/backend-protocol: "HTTPS" ## Since its having it's on ssl certificate.
spec:
  tls:
    - hosts:
      - cas-qa.platinumecn.com
      secretName: oms-ssl
  rules:
  - host: cas-qa.platinumecn.com
    http:
      paths:
      - pathType: Prefix
        path: /
        backend:
          service:
            name: ms-pats-users-svc-qa
            port: 
              number: 9444

```


### Useful Commands
> To list all pods
```shell
$ kubectl get pods -o wide
```
> To list all services
```shell
$ kubectl get svc -o wide
```
> To list ingress
```shell
$ kubectl get ing -o wide
```
> To rollout new deployment
```shell
$ kubectl rollout restart deploy <deployment-name>
```

#### Notes
- we are using Multi-Staged Dockerfile.
- we have created an tls secret in kubernetes for TLS termination in Ingress Controller.


```shell
   15  clear
   16  javac -cp ./com.ibm.mq.allclient-9.2.0.0.jar:./javax.jms-api-2.0.1.jar com/ibm/mq/samples/jms/JmsPutGet.java
   17  vim com/ibm/mq/samples/jms/JmsPutGet.java
   18  clear
   19  javac -cp ./com.ibm.mq.allclient-9.2.2.0.jar:./javax.jms-api-2.0.1.jar com/ibm/mq/samples/jms/JmsPutGet.java
   20  java -Djavax.net.ssl.trustStore=start_cert.p12 -Djavax.net.ssl.trustStorePassword=Welcome1 -Dcom.ibm.mq.cfg.useIBMCipherMappings=false -cp ./com.ibm.mq.allclient-9.2.2.0.jar:./javax.jms-api-2.0.1.jar:. com.ibm.mq.samples.jms.JmsPutGet
   21  java -Djavax.net.ssl.trustStore=start_cert.p12 -Djavax.net.ssl.trustStorePassword=Welcome1 -Dcom.ibm.mq.cfg.useIBMCipherMappings=false -cp ./com.ibm.mq.allclient-9.2.2.0.jar:./javax.jms-api-2.0.1.jar:. com.ibm.mq.samples.jms.JmsPutGet
   22  clear
   23  history 

```