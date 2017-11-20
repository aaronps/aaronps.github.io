# kafka

https://kafka.apache.org/

## Setup

Download the tgz, uncompress

1. start zookeper:
	* config is in `config/zookeper.properties`
		* may need to change dataDir for windows
		* port is defined here too
	* `bin\windows\zookeper-server-start config\zookeper.properties`
	* note: stopping zookeper also stops all the clients connected to it (at least the kafka console-consumer/producer I was running)
	* note: zookeeper-shell to do some operations on the server
	
2. start kafka:
	* config is in config/server.properties:
		* may need to set broker.id for each broker
		* may need to change log.dirs for windows
		* zookeper.connect to set the port configured on zookeper
		* If want to change the port (default is 9092) change "listeners"
		* If want to be able to delete topics set: delete.topic.enable=true
	* bin\windows\kafka-server-start.bat config\server.properties

## Setup 1.0.0

Download 1.0.0.tgz, uncompress somewhere, also uncompress included docs for reference (site-docs)




## Commands

Handle topics (--zookeeper always needed):
```
	bin\windows\kafka-topics --zookeeper localhost:2181 <cmd>
		--list -> list thje topics
		--create --topic <topic_name> --replication-factor 1 --partitions 1
		--delete --topic <topic_name> (note it requires config)
		--describe --topic <topic_name>
```

Send messages:
```
	bin\windows\kafka-console-producer --broker-list localhost:9092 --topic <name>
	(this will read from stdin and publish on topic)
```
	
Receive messages
```
	bin\windows\kafka-console-consumer --bootstrap-server localhost:9092 --topic <name>
	* --from-beginning if want to receive starting from the earliest present in the log
	* --property print.key=true if want to see the keys
	* --property key.separator="<separator>" to set the separator, default \t 
```

### Recover after kafka-server-stop

After stopping with this command, when trying to start again it complains some file.timeindex (from a topic) is used by another program:

	* stopping zookeeper and starting again doesn't help
	* seems related to https://issues.apache.org/jira/browse/KAFKA-6044
	* To fix just delete the kafka-logs data folder…
	
### Start multiple nodes in a cluster

Use different configuration files (config/server.properties) with different id and different listener port:
```
	broker.id = 1
	listeners=PLAINTEXT://:9093
	log.dir=/another/dir/for/this/one
```

Now when creating a topic we could add a replication factor bigger than one

## Connectors Are Kings

Maybe
	
## Usage notes

Consumer should poll() at least once every session.timeout.ms or it will get CommitFailedException, this is also usefull if the binary fails to work or hangs somewhere.
 
