# Traffic Data Stream Analysis 
#### Using Kafka, Spark, Docker in Python

This project demonstrates how to analyze traffic data using Apache Kafka and Apache Spark within a Dockerized environment. The focus is on connecting Spark Structured Streaming to Kafka for processing real-time data and performing various operations like windowed aggregations.

## Table of Contents

- [Project Overview](#project-overview)
- [Run a Container and Execute Bash Commands](#Run-a-container-and-execute-bash-commands)
- [Setup Test Environment](#Setup-test-environment)
  - [Inside the Kafka Container](#Inside-the-kafka-container)
  - [Inside the Spark Container](#Inside-the-spark-container)
- [Setup Traffic Sensor Topic](#traffic_sensor_topic)


## Project Overview

This project includes:
- Populating, consuming, and processing Kafka topics using Spark jobs.
- Understanding different output modes specific to streaming applications and how each can be used.
- Performing aggregations with time windows, a key capability of stream processing.

## Run-a-container-and-execute-bash-commands

To start the containers and access them via bash:

```bash
docker compose up
docker ps -a
docker exec -it <container_id or name> bash
```


## Setup-test-environment
### Inside-the-kafka-container:

```bash

kafka-topics.sh --create --replication-factor 1 --bootstrap-server localhost:9092 --topic test_topic

kafka-topics.sh --list --bootstrap-server localhost:9092

kafka-console-producer.sh --bootstrap-server localhost:9092 --topic test_topic --property "parse.key=true" --property "key.separator=:"
```


### Inside-the-spark-container:
  ```bash
cd /src
### Convert the jsons into parquet file
# Unprotect the folder
sudo chmod -R 777 data/ 

```


```bash
## fisrt tes streame
spark-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.3.0 /src/read_test_stream.py

#### JSON to parquet job
spark-submit --deploy-mode client --master spark://spark:7077 --driver-memory 2G --executor-memory 2G transform_json_to_parquet.py```

### Setup traffic_sensor topic
```
### Inside the kafka container :
#### traffic_sensor_topic
```bash
kafka-topics.sh --create --replication-factor 1 --bootstrap-server localhost:9092 --topic traffic_sensor

kafka-console-consumer.sh --topic traffic_sensor --bootstrap-server localhost:9092
```

### Run Other Scripts to submit spark grouping to see the spark structured streaming using windowing

```bash
### Test write
spark-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.3.0 streaming/insert_traffic_topic.py

### Test read
spark-submit --packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.3.0 streaming/consume_traffic_topic.py

### Execute a general job
spark-submit --master spark://spark:7077 --packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.3.0 <<job.py>>
```


