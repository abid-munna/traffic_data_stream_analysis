## Traffic Data Streame Analysis using Kafja Spark(pyspark), Docker 
Take a look into spark structure streaming that can connect to Kafka and process its messages, 
- how to populate, consume, and process Kafka topics using Spark jobs. 
- Different output modes, something specific to stream applications, and how each one can be used.
- Aggregations with time windows, one of the main capabilities of stream processing.

### Run a container and execute bash commands inside it 
```bash
docker compose up
docker ps -a
docker exec -it <container_id or name> bash
```

### Setup test environment
### Inside the kafka container:

```bash

kafka-topics.sh --create --replication-factor 1 --bootstrap-server localhost:9092 --topic test_topic

kafka-topics.sh --list --bootstrap-server localhost:9092

kafka-console-producer.sh --bootstrap-server localhost:9092 --topic test_topic --property "parse.key=true" --property "key.separator=:"
```


### Inside the spark container:
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
#### traffic_sensor topic:
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
