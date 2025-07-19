## Kafka Quick Start

Content is from [reference - Kafa quick start](https://kafka.apache.org/quickstart)

1. download kafka console from [here](https://www.apache.org/dyn/closer.cgi?path=/kafka/4.0.0/kafka_2.13-4.0.0.tgz)

2. unzip kafka 

```
tar -xzf kafka_2.13-4.0.0.tgz
cd kafka_2.13-4.0.0
```
3. Generate UUID
```
KAFKA_CLUSTER_ID="$(bin/kafka-storage.sh random-uuid)"
```
4. Format Log Directories

```
bin/kafka-storage.sh format --standalone -t $KAFKA_CLUSTER_ID -c config/server.properties
```

5. Start Kafka Server
```
bin/kafka-server-start.sh config/server.properties
```

6. Topic Creation (i.e. topic name : input-topic)
```
bin/kafka-topics.sh --create --topic input-topic --bootstrap-server localhost:9092
```

7. Console Producer (topic : input-topic, broker: localhost:9092)
```
bin/kafka-console-producer.sh --topic input-topic --bootstrap-server localhost:9092
```

8. Console Consumer(topic : input-topic, broker: localhost:9092)
```
bin/kafka-console-consumer.sh --topic input-topic --from-beginning --bootstrap-server localhost:9092

```


