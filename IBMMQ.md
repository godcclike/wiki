### Change User
> login with mqm user `su - mqm`


### Create the Queue Manager
```shell
$ . /opt/mqm/bin/setmqenv -n Installation1
$ /opt/mqm/bin/crtmqm -lc -lf 65535 -lp 3 -ls 2 -u SYSTEM.DEAD.LETTER.QUEUE MQ_RATE_MANAGER
$ /opt/mqm/bin/strmqm MQ_RATE_MANAGER
```

### Create base objects within the Queue Manager

```shell
$ . /opt/mqm/bin/setmqenv -n Installation1
$ /opt/mqm/bin/runmqsc MQ_RATE_MANAGER << EOF

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

#DEFINE TOPIC(OMS.OCBC.RATE.TOPIC) TOPICSTR(RATETOPIC)
#DEFINE TOPIC(OMS.OCBC.CHANGESOURCE.TOPIC) TOPICSTR(CHANGESOURCETOPIC)

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
```shell
su - mqm
cd /opt/mqm/bin
strmqweb
```