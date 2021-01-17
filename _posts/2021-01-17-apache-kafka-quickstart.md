## Introduction 
[Apache Kafka](https://kafka.apache.org) is a distributed publish-subscribe messaging system designed to be fast, scalable, and durable. Kafka queues messages from producers and sends it to consumers. It is currently very popular. Can be used to communicate microservices in asynchronous manner but also to activity tracking, metrics, logging or stream processing.

Kafka cluster could contain one broker or many broker instances. Current version (2.7.0) of Kafka requires [Apache Zookeeper](https://zookeeper.apache.org) to manage and coordinate Kafka broker but per this [announcement](https://www.confluent.io/blog/removing-zookeeper-dependency-in-kafka/) you may expect that Zookeeper Dependency will be removed in the future.

## Instalation 
Kafka binaries are available for download from https://kafka.apache.org/downloads 
If you start from completely beginning choose the file with higher Scala version.

Downloaded file should be extracted and unzipped by the command:
```bash
tar -xzf kafka_2.13-2.7.0.tgz
cd kafka_2.13-2.7.0
```
Root folder contains folders 
- bin - with scripts to manage kafka instance for linux and windows
- config - with configuration of zookeeper and kafka broker
- libs - with Kafka and third party dependencies binaries
- logs
- site-docs - zipped documentation

Kafka is written in Scala and your local environments needs only to have installed Java 8+. Scala lib files are stored inside `lib` folder.

## Start and stop the Kafka environment
Zookeeper instance should be started upfront. In case simple use case when only one instance of Zookeeper is needed to work we don't have to modify anything in default settings `config\zookeper.properties`. It is only worth to notice that by default zookeeper exposes port `2181` for clients.

```bash
# Start the Zookeeper service on Linux
bin/zookeeper-server-start.sh config/zookeeper.properties

# Start the Zookeeper service on Windows
bin\windows\zookeeper-server-start.bat config\zookeeper.properties
```

Once Zookeeper instance has started we may start Kafka broker. Default settings at `config\server.properties` are sufficient to development purpose especially when we are not going to start more brokers on the same machine. Therefore in a new console tab execute:

```bash
# Start the Kafka broker on Linux
bin/kafka-server-start.sh config/server.properties

# Start the Kafka broker on Windows
bin\windows\kafka-server-start.bat config\server.properties
```

Once you want to terminate started services stop Zookeeper and Kafka simply with `Ctrl+C` or by executing `bin\zookeeper-server.stop.sh` and `bin\kafka-server-stop.sh`. Additionaly you may delete any data of your local evironment by 
```bash
rm -rf /tmp/kafka-logs /tmp/zookeeper
```

More info about quickstart with examples how to create topic, produce and consume messages you may find [here](https://kafka.apache.org/quickstart)

### Cluster with many Brokers on local instance
Sometimes we want to have more brokers to evaluate production environment and seeing how replica works. It is straightforward to have many local instances by cloning the `server.properties` config file and change 3 properties inside:

```properties
############################# Server Basics #############################

# The id of the broker. This must be set to a unique integer for each broker.
broker.id=0

############################# Log Basics #############################

# A comma separated list of directories under which to store log files
log.dirs=/tmp/kafka-logs

############################# Socket Server Settings #############################

# The address the socket server listens on. It will get the value returned from 
# java.net.InetAddress.getCanonicalHostName() if not configured.
#   FORMAT:
#     listeners = listener_name://host_name:port
#   EXAMPLE:
#     listeners = PLAINTEXT://your.host.name:9092
listeners=PLAINTEXT://:9092
```

Broker id should be unique within the cluster, log.dirs also should provide unique path and broker port should be available.
New broker can be started by command `bin/kafka-server-start.sh config/{new-broker-name}.properties`





