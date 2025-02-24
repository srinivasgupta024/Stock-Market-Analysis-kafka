# Setting up Apache Kafka on an EC2 Instance

## Step 1: Create an EC2 Instance
1. Launch an EC2 instance (Amazon Linux, Ubuntu, or any preferred OS,I used Amazon Linux).
2. Configure security groups to allow SSH (port 22) and Kafka/Zookeeper ports (e.g., 2181 for Zookeeper, 9092 for Kafka).
3. SSH into the instance:
   ```sh
   ssh -i your-key.pem ec2-user@your-ec2-public-ip
   ```

## Step 2: Install Java and Kafka
Kafka requires Java to run, so install Java first:
```sh
sudo apt update   # For Ubuntu
sudo yum update   # For Amazon Linux
sudo apt install -y openjdk-11-jdk  # Ubuntu
sudo yum install -y java-11-openjdk  # Amazon Linux
java -version
```

Download and extract Kafka:
```sh
wget https://downloads.apache.org/kafka/3.0.0/kafka_2.12-3.7.2.tgz
```
```sh
 tar -xvf kafka_2.12-3.7.2.tgz
 cd kafka_2.12-3.7.2
```

## Step 3: Start Zookeeper

```sh
bin/zookeeper-server-start.sh config/zookeeper.properties &
```

## Step 4: Start Kafka Server
Modify Kafka's configuration to use the EC2 public IP:
```sh
sed -i 's/#advertised.listeners=PLAINTEXT:\/\/your.host.name:9092/advertised.listeners=PLAINTEXT:\/\/your-ec2-public-ip:9092/' config/server.properties
```
or 
```sh
bin/kafka-server-start.sh config/server.properties
```
Start the Kafka server:
```sh
bin/kafka-server-start.sh config/server.properties &
```

## Step 5: Create a Kafka Topic
```sh
export KAFKA_HEAP_OPTS="-Xmx256M -Xms128M"
```
Create a topic named `test-topic`:
```sh
bin/kafka-topics.sh --create --topic test-topic --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1
```
Verify topic creation:
```sh
bin/kafka-topics.sh --list --bootstrap-server localhost:9092
```

## Step 6: Start a Producer
```sh
bin/kafka-console-producer.sh --topic test-topic --bootstrap-server localhost:9092
```
Type messages in the console and press Enter to send.

## Step 7: Start a Consumer
```sh
bin/kafka-console-consumer.sh --topic test-topic --from-beginning --bootstrap-server localhost:9092
```
You should see the messages produced earlier.
Kafka is now successfully running on the EC2 instance!

