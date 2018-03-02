# confluent-kafka
ansible role and tests for confluent-kafka 

* Installs a 3 node Vagrant cluster using CentOS 7, although it will work with any debian based release easily
* Deploys a Confluent Kafka (Platform) cluster supported by multi-node Zookeeper
* Happens as a single role for kafka and zookeeper because this is what is supported by the confluent release
* Enables SSL for authentication and broker traffic encryption
* Uses systemd for kafka and zookeeper services


# Prerequisites

## On OSX
```
brew cask install virtualbox
brew cask install vagrant
brew install ansible
vagrant plugin install vagrant-hostsupdater
vagrant plugin install vagrant-vbguest
```

Generate the SSL certs and stores (requires OpenSSL and keytool)

```
cd secrets
./create-certs.sh
```

## Trying it on Vagrant

'vagrant up'


## Testing

Create a test topic
```
/usr/bin/kafka-topics --create --zookeeper 192.168.33.10:2181 --replication-factor 3 --partitions 1 --topic test
/usr/bin/kafka-topics --describe --zookeeper localhost:2181 --topic test
```

Test Kafka's talking SSL on TCP:9092

`openssl s_client -debug -connect localhost:9092 -tls1`

produce on Broker1

`/usr/bin/kafka-console-producer --broker-list localhost:9092 --topic test --producer.config  /vagrant/secrets/host.producer.ssl.config`

listen on Broker2

`/usr/bin/kafka-console-consumer --bootstrap-server localhost:9092 --topic test --consumer.config  /vagrant/secrets/host.consumer.ssl.config`

## Building a cluster on AWS

The cloudformation script will create a cluster of the size in the cloudformation script; however for this to happen properly you need to create an ENI for each broker and an EBS for each broker they need tags like this:

For the ENI's

| Key | Value |
| ------------- | ------------- |
| Name | kafka-eni |
| brokerid | [integer 1-9 for broker] |
| kafka | broker[same integer for broker]|


For the EBS's

| Key | Value |
| ------------- | ------------- |
| brokerid | [integer 1-9 for broker] |
| kafka | broker[same integer for broker]|




## References


https://kafka.apache.org/documentation/#security

http://docs.confluent.io/current/kafka/ssl.html

## Credits

create-certs.sh is Confluent's and updated to not prompt and output for the hosts in this example.

https://github.com/confluentinc/cp-docker-images/blob/v3.2.1/examples/kafka-cluster-ssl/secrets/create-certs.sh