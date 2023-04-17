### Change User
| Syntax | Description |
| ----------- | ----------- |
| `sudo su - mqm` | login with mqm |


### Create the Queue Manager
```shell
. /opt/mqm/bin/setmqenv -n Installation1` |
/opt/mqm/bin/crtmqm -lc -lf 65535 -lp 3 -ls 2 -u SYSTEM.DEAD.LETTER.QUEUE MQ_RATE_MANAGER` | 
/opt/mqm/bin/strmqm MQ_RATE_MANAGER` |
```

### Create base objects within the Queue Manager
```shell
. /opt/mqm/bin/setmqenv -n Installation1
/opt/mqm/bin/runmqsc MQ_RATE_MANAGER << EOF

/opt/mqm/bin/runmqsc MQ_RATE_MANAGER_5 < "/home/ubuntu/5-ibmq.mqsc"

ALTER QMGR MAXMSGL(4194304)
ALTER QL(SYSTEM.CLUSTER.TRANSMIT.QUEUE) MAXMSGL(4194304)
DEFINE LISTENER(TCPLISTENER.MQ_RATE_MANAGER) TRPTYPE(TCP) CONTROL(QMGR) PORT(1414)
START LISTENER(TCPLISTENER.MQ_RATE_MANAGER)
dis lsstatus(TCPLISTENER.MQ_RATE_MANAGER)


DEFINE CHANNEL(OMS.RATE.CHANNEL) CHLTYPE(SVRCONN) MCAUSER('mqm') MAXMSGL(4194304) DESCR('Channel for incoming clients')
DISPLAY CHANNEL(OMS.SI.MUREX.CHANNEL)
ALTER QMGR CHLAUTH(disabled)
REFRESH SECURITY TYPE(CONNAUTH)
ALTER AUTHINFO(SYSTEM.DEFAULT.AUTHINFO.IDPWOS) AUTHTYPE(IDPWOS) CHCKCLNT(NONE)
REFRESH SECURITY TYPE(CONNAUTH)
dis AUTHINFO(SYSTEM.DEFAULT.AUTHINFO.IDPWOS)

DEFINE TOPIC(OMS.OCBC.RATE.TOPIC) TOPICSTR(RATETOPIC)
DEFINE TOPIC(OMS.OCBC.CHANGESOURCE.TOPIC) TOPICSTR(CHANGESOURCETOPIC)

DEFINE QL(ah.oms.adaptor.price.source.queue)

end
EOF
```
### Send message
```shell
printf "%s\n\n" TestMessage1 | /opt/mqm/samp/bin/amqsput AH.OMS.ADAPTOR.PRICE.SOURCE.QUEUE MQ_RATE_MANAGER
```
### Send message using ssl
```shell
java -Djavax.net.ssl.trustStore=clientTruststore.p12 -Djavax.net.ssl.trustStorePassword=Welcome1 -Djavax.net.ssl.keyStore=clientKeystore.p12 -Djavax.net.ssl.keyStorePassword=Welcome1 -Dcom.ibm.mq.cfg.useIBMCipherMappings=false -cp ./com.ibm.mq.allclient-9.2.2.0.jar:./javax.jms-api-2.0.1.jar:. com.ibm.mq.samples.jms.JmsPutGet
```

### Start ibmmq web
| Syntax | Description |
| ----------- | ----------- |
| `su - mqm` |
| `cd /opt/mqm/bin` |
| `strmqweb` |

### To see the contents of the SSL certificate
| Syntax | Description |
| ----------- | ----------- |
| `keytool -v -list -keystore clientTruststore.p12` | to see the content of the cert like expiry date |
| `base64 clientTruststore.p12` | to decrypt the public/private key values |

### To generate new mutual SSL cert
| Syntax | Description |
| ----------- | ----------- |
| `su - mqm` | login to mqm user |
| `cd /home/ubuntu` | 
| `sudo rm -rf client*` | remove previous keystore and truststore |  
| `cd /var/mqm/qmgrs/<queue-manager-name>/ssl` | move into the ssl directory for the queue manager |
| `sudo rm - rf *` | remove everything here to create new |
| `runmqakm -keydb -create -db key.kdb -pw Welcome1 -stash` | Create a keystore (a .kdb file) using the MQ security tool |
| `sudo chgrp mqm *` | change owner of all files |
| `sudo chmod 640 *` | change permission of all files |
| `runmqakm -cert -create -db key.kdb -stashed -dn "cn=qm,o=ibm,c=uk" -label ibmwebspheremqqm1` | create a self-signed certificate and private key and put them in the keystore |
| `runmqakm -cert -extract -label ibmwebspheremqqm1 -db key.kdb -stashed -file QM.cert` | extract the queue manager certificate |
| `cd /home/ubuntu` | change to home directory |
| `sudo keytool -keystore clientTruststore.p12 -storetype pkcs12 -storepass Welcome1 -importcert -file /var/mqm/qmgrs/<queue-manager-name>/ssl/QM.cert -alias server-certificate` | create a client truststore and import the queue manager certificate |
| `sudo keytool -genkeypair -keyalg RSA -alias client-key -keystore clientKeystore.p12 -storepass Welcome1 -storetype pkcs12` | create a public and private key pair |
| `keytool -list -v -keystore clientKeystore.p12` | to see the content of the cert like expiry date |
| `sudo keytool -export -alias client-key -file clientCertificate.crt -keystore clientKeystore.p12` | Extract the client certificate to the file clientCert.crt |
| `cd /var/mqm/qmgrs/<queue-manager-name>/ssl` | move into the ssl directory for the queue manager |
| `runmqakm -cert -add -db key.kdb -stashed -label ibmwebspheremqapp -file /home/ubuntu/clientCertificate.crt` | add that certificate to the queue manager's key repository, so the server knows that it can trust the client |
| `runmqakm -cert -list -db key.kdb -stashed` | List the certificates in the key repository |
| `. /opt/mqm/bin/setmqenv -n Installation1` |
| `/opt/mqm/bin/runmqsc <Queue Manager name>` |
| `REFRESH SECURITY(*) TYPE(SSL)` |
| `exit` |
| `cd /home/ubuntu` | go back to where the keystore and truststore is located |
| `base64 clientTruststore.p12` | get the decoded value for keystore and truststore then paste it in the OCBC-Kube_config/custom-service/service-kustomize/ssl-secrets repo |
| `base64 clientKeystore.p12` | get the decoded value for keystore and truststore then paste it in the OCBC-Kube_config/custom-service/service-kustomize/ssl-secrets repo |
