# Kafka Streaming Pipeline on AWS

## Overview
This project sets up a Kafka-based data streaming pipeline on AWS, utilizing an EC2 instance for Kafka deployment, Jupyter notebooks for development, and AWS services like S3, Glue, and Athena for data storage and querying. The system processes real-time data from an Excel file, streams it via Kafka, and stores the transformed data in S3, making it accessible for analysis using AWS Glue and Athena.

![image](https://github.com/user-attachments/assets/c4a47e3e-4dbd-4244-bec3-0f54d3b775e9)

## Steps

### 1. EC2 Setup
- Launched an EC2 instance (Amazon Linux) with necessary configurations.
- Installed Java (since Kafka requires Java to run) and Kafka.
- Downloaded and extracted the Kafka tarball.
- Started the Zookeeper service, which manages and coordinates Kafka brokers.
- Configured the Kafka server to use the public IP of the EC2 instance for accessibility.
- Started the Kafka broker and verified that the service was running correctly.
- Created a Kafka topic for message streaming.

### 2. Jupyter Notebook Setup
- Set up Jupyter notebooks to interact with Kafka for both producer and consumer operations.
- Installed required Python dependencies such as `kafka-python` and `boto3`.

### 3. Kafka Producer
- Loaded a dataset from an Excel file using Pandas.
- Simulated real-time stock market data streaming by reading rows iteratively.
- Used Kafka producer to send each row of data as a message to the Kafka topic.
- Verified successful message transmission using Kafka logs and consumer testing.

### 4. Kafka Consumer
- Subscribed to the Kafka topic to consume messages in real time.
- Parsed the received messages and transformed them into structured JSON format.
- Stored the processed JSON data in an Amazon S3 bucket for further analysis.

### 5. AWS Integration
- Set up an AWS Glue Crawler to scan the JSON files in S3 and create a metadata catalog.
- Defined a schema in AWS Glue for structured querying.
- Created an AWS Glue database and table from the cataloged S3 data.
- Used Amazon Athena to run SQL queries on the ingested data directly from S3.

## Conclusion
This architecture provides a robust and scalable data streaming pipeline that enables real-time data ingestion, processing, and querying. Kafka ensures efficient message handling, while AWS services like S3, Glue, and Athena facilitate seamless storage and analysis. The implementation allows for easy expansion to accommodate additional data sources, analytics, and machine learning applications.
