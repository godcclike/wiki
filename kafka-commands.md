# **Kafka Commands**

**To view all topics**
    
    kafka-topics.sh --bootstrap-server [broker0:9092,broker1:9092..] --describe

**To delete a topic**

    kafka-topics.sh --bootstrap-server [broker0:9092,broker1:9092..] --delete --topic my_topic