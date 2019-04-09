## Kafka Producer
* topic 생성 후 producer 수행 > 메세지 입력
```
[training@localhost ~]$ kafka-topics --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic weblogs
Created topic "weblogs".
[training@localhost ~]$ kafka-topics --list --zookeeper localhost:2181
weblogs
[training@localhost ~]$ kafka-topics --describe weblogs --zookeeper localhost:2181
Topic:weblogs	PartitionCount:1	ReplicationFactor:1	Configs:
	Topic: weblogs	Partition: 0	Leader: 0	Replicas: 0	Isr: 0
[training@localhost ~]$ kafka-console-producer --broker-list localhost:9092 --topic weblogs

test weblog entry 1



^C[training@localhost ~]$
[training@localhost ~]$ kafka-console-producer --broker-list localhost:9092 --topic weblogs

test weblog entry 1
test kafka    

test weblog entry 2
test kafka 2

```

## Kafka Consumer
* 메세지를 받은 것을 확인 가능
```
[training@localhost ~]$ kafka-console-producer --broker-list localhost:9092 --topic weblogs
^C[training@localhost ~]$ kafka-console-consumer --zookeeper localhost:2181 --toc weblogs --from-beginning

test weblog entry 1




test weblog entry 1
test kafka
^CProcessed a total of 8 messages
```
### --from-beginning 옵션을 제외하고 다시 실행
```
[training@localhost ~]$ kafka-console-consumer --zookeeper localhost:2181 --topic weblogs


test weblog entry 2
test kafka 2

```
