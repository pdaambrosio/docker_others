# Zookeeper + Kafka

Docker Compose setup for a 3-node Zookeeper cluster with a 3-broker Kafka cluster.

## Stack

- Zookeeper 3.5 (3 nodes: zookeeper01, zookeeper02, zookeeper03)
- Kafka (3 brokers: kafka01, kafka02, kafka03)

## Usage

```bash
docker-compose up -d
```

| Service | Port |
|---|---|
| Zookeeper 01 | 2181 |
| Zookeeper 02 | 2182 |
| Zookeeper 03 | 2183 |
| Kafka 01 | 9092 |
| Kafka 02 | 9093 |
| Kafka 03 | 9094 |
