# Spark-Socket Streaming


## Architecture

![](assets/System_architecture.png)


## How to Run

- Set up Confluent (Kafka) Cluster and Configuration

    - Go to [Confluent Cloud](https://confluent.cloud/)
    - Create an environment with default cluster
    - Create an API Key for your Cluster and replace it with `sasl.username` and `sasl.password` in [config.py](src/config/config.py)
    - Get the Bootstrap server URL for your confluent kafka cluster and replace it with `bootstrap.servers` in [config.py](src/config/config.py)
    - Create an Schema Registry API Key and replace it with `basic.auth.user.info` in format `<api_key>:<secret_jey>` in [config.py](src/config/config.py)
    - Get the Schema Registry URL for your confluent kafka cluster and replace it with `schema_registry` in [config.py](src/config/config.py)
    - Create an Topic name `customers_review` with schema (AVRO Based) from [here](src/schema/reviews.schema.avsc)
    
    - Replace `api_key` in [config.py](src/config/config.py) with OPENAI-API Key.


- Download the Data
    - Get the Yelp dataset from [here](https://www.yelp.com/dataset/download) in `JSON` format.
    - Unzip the dataset and place the unzipped folder named `yelp_dataset` in `datasets`

- Run the services
 
```bash
cd src
```

```bash
docker-compose up -d --build
```

- Start the Socket Server

```bash
docker exec -it spark-master python jobs/streaming-sockets.py
```

- Start the Spark Streaming Job

```bash
docker exec -it spark-master spark-submit \
--master spark://spark-master:7077 \
--packages org.apache.spark:spark-sql-kafka-0-10_2.12:3.5.1 \
jobs/spark-streaming.py
```