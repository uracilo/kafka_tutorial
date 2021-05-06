Download the binary

- [Kafka](https://kafka.apache.org/downloads) - Apache Kafka



# Arrancamos Zookeeper
<KAFKA_HOME>

```sh
/bin/zookeeper-server-start.sh config/zookeeper.properties
```
# Arrancamos Kafka
<KAFKA_HOME>
```sh
/bin/kafka-server-start.sh config/server.properties
```

# Creamos nuestro primer topic 'test'
<KAFKA_HOME>
```sh
/bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic test
```
# Comprovamos que existe
<KAFKA_HOME>
```sh
/bin/kafka-topics.sh --list --bootstrap-server localhost:9092
```


# Productor
<KAFKA_HOME>
```sh
/bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
```
# Consumidor
<KAFKA_HOME>
```sh
/bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
```
# App 

```sh
const kafka = require('kafka-node');

const client = new kafka.KafkaClient({kafkaHost: '127.0.0.1:9092'});

/* Consumidor */
var consumer = new kafka.Consumer(client, [ { topic: 'test' } ]);

consumer.on('message', function (message) {
    	console.log(message);
	});

/* Productor */
var producer = new kafka.Producer(client);

producer.on('ready', function () {

	setInterval(function() {
  		producer.send( [ { topic: "test", messages: "Mensaje automático cada 5 seg." } ], function (err,data) {} );
		}, 5000);


	});
```
