# Real-Time Stock Price Analysis with Kafka and AWS

A brief description of your project.

## Table of Contents

- [Project Overview](#project-overview)
- [Architecture](#architecture)
- [Setup](#setup)
- [Using AWS Glue](#using-aws-glue)
- [Querying with Athena](#querying-with-athena)
- [Connecting to EC2 Instance](#connecting-to-ec2-instance)
- [Produce & Consume Data](#produce--consume-data)
- [License](#license)

## Project Overview

This project leverages Kafka and AWS services to provide a robust framework for real-time stock price analysis. Key features include:

- **Data Extraction**: Pulls data from the Yahoo Finance API.
- **Real-Time Streaming**: Streams stock price data via Kafka.
- **Data Management**: Utilizes AWS S3 for storage and AWS Glue for schema inference.
- **Data Analysis**: Enables querying capabilities using AWS Athena.
- **Visualization**: Connects to Power BI for creating insightful dashboards.

## Architecture

1. **Data Ingestion**: Kafka streams stock price data for real-time processing.
2. **Storage**: AWS S3 stores the streamed data files.
3. **Schema Management**: AWS Glue infers data schemas and catalogs them.
4. **Data Querying**: AWS Athena provides SQL-like querying for analysis.
5. **Visualization**: Power BI connects to Athena for dashboard creation.

## Setup

Instructions for setting up the project. This can include:

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/yourusername/your-repository.git
   cd your-repository
   ```

2. **Install Dependencies**:
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   pip install -r requirements.txt
   ```

## Connecting to EC2 Instance(check also the command_kafka.txt file)

Steps for connecting to your EC2 instance:

1. **Navigate to your project directory**

Run the stock-market.pem file (after you have created the EC2 instance)
  ```bash
   cd /c/Users/Thanos/Documents/Stock_Price_Kafka
   ```

cd /c/Users/Thanos/Documents/Stock_Price_Kafka
2. **SSH into Your EC2 Instance**:
   ```bash
    ssh -i /path/to/your-key.pem ec2-user@your-ec2-public-dns
   ```

3. **Download dependencies (kafka, java)**:
  ```bash
    wget https://downloads.apache.org/kafka/3.8.0/kafka_2.13-3.8.0.tgz
    tar -xvf kafka_2.13-3.8.0.tgz

    sudo apt update
    sudo yum install openjdk-8-jdk
  ```


4. **Start Zookeeper(but first navigate to Kafka directory)**:
    ```bash
    cd kafka_2.13-3.8.0

    bin/zookeeper-server-start.sh config/zookeeper.properties
   ```

5. **Start Kafka-server** (Duplicate the session & enter in a new console):
   ```bash
   export KAFKA_HEAP_OPTS="-Xmx256M -Xms128M"

   cd kafka_2.13-3.8.0

   bin/kafka-server-start.sh config/server.properties
   ```

6. **Create the topic (if needed):
   ```bash
   cd kafka_2.13-3.8.0
   bin/kafka-topics.sh --create --topic <topic_name> --bootstrap-server <EC2-IP-Address>:9092 --replication-factor 1 --partitions 1
   ```

7. **Start Producer**:
   ```bash
   bin/kafka-console-producer.sh --topic <topic_name> --bootstrap-server <EC2-IP-Address>:9092
   ```

8. **Start Consumer**:
   ```bash
   cd kafka_2.12-3.8.0
   bin/kafka-console-consumer.sh --topic <topic_name> --bootstrap-server <EC2-IP-Address>:9092

## Produce & Consume data:
- the KafkaProducer.ipynb sets up a Kafka producer that continuously fetches historical stock data and sends it to a Kafka topic (stock-prices-test). Note that in this project the fetch_realtime_data function is not been used because we had first to fetch the historical data. 
- the KafkaConsumer.ipynb sets up a Kafka consumer that continuously reads messages from the stock-prices-test topic and writes each message as a separate JSON file to Amazon S3.

## Power BI dashboard:
- Since we do not have access to Power BI Service, we can only export the Power BI dashboard as a png image in the powerbi-dashboard subdirectory of this project.
- The dashboard provides a comprehensive view of the top-4 companies stock performance over time in the technology sector. It offers key metrics such as closing price, return percentage, volume, and a summary of these metrics across different time periods (Year, Quarter, and Month). You can easily adjust the time range and select for which you want to display data for.

## License

This project is licensed under the MIT License.