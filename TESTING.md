# confluent-kafka - testing an AWS installation

## checking zookeeper




## create a topic

kafka-topics --zookeeper localhost:2181 --create --topic testing --partitions 2 --replication-factor 2

## produce some data

kafka-console-producer --broker-list localhost:9092 --topic testing

then type in some messages

## consume some data

kafka-console-consumer --bootstrap-server localhost:9092 --topic testing