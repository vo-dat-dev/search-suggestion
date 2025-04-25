# Search Suggestion System

A real-time search suggestion system built with Apache Kafka and Apache Flink.

## 🏗️ Architecture

The system consists of two main components:

- Apache Kafka for message streaming
- Apache Flink for stream processing

### Infrastructure Components

#### Kafka Stack:

- **Zookeeper**: Coordination service (port 2181)
- **Kafka Broker**: Message broker (ports 9092, 9101)
- **Schema Registry**: Manages Avro schemas (port 8081)
- **Kafka Connect**: Data integration framework (port 8083)
- **Control Center**: Management UI for Kafka (port 9021)
- **Kafka Producer**: Custom service for message production

#### Flink Stack:

- **JobManager**: Flink cluster coordinator (port 8081)
- **TaskManager**: Worker node that executes tasks

## 🚀 Getting Started

### Prerequisites

- Docker
- Docker Compose
- At least 8GB of RAM available for Docker

### Environment Setup

1. Create the required network:

```bash
docker network create easymlops_network
```

2. Start the Kafka stack:

```bash
docker-compose -f docker-compose.kafka.yml up -d
```

3. Start the Flink cluster:

```bash
docker-compose -f docker-compose.flink.yml up -d
```

### Verifying the Setup

1. Check Kafka Control Center:

   - URL: http://localhost:9021
   - Use this to monitor Kafka topics, connectors, and overall cluster health

2. Check Flink Dashboard:
   - URL: http://localhost:8081
   - Use this to monitor Flink jobs and cluster status

## 📊 Service Ports

| Service          | Port | Purpose                         |
| ---------------- | ---- | ------------------------------- |
| Zookeeper        | 2181 | Cluster coordination            |
| Kafka Broker     | 9092 | Client communication            |
| Schema Registry  | 8081 | Schema management               |
| Kafka Connect    | 8083 | Data integration                |
| Control Center   | 9021 | Kafka management UI             |
| Flink JobManager | 8081 | Flink web UI and REST endpoints |

## 🛠️ Configuration

### Kafka Configuration

The Kafka stack is configured with:

- Single broker setup
- Schema Registry for data consistency
- Kafka Connect for data integration
- Control Center for monitoring and management

Key configurations:

- Replication factor: 1 (development setup)
- PLAINTEXT listener configuration
- Integrated metrics reporting

### Flink Configuration

The Flink cluster is configured with:

- 1 JobManager
- 1 TaskManager (scalable)
- 2 task slots per TaskManager
- 1.6GB memory for JobManager
- 1.7GB memory for TaskManager

## 🔧 Development

When developing Flink jobs:

- Use `broker:29092` as your Kafka bootstrap server address
- Schema Registry is available at `http://schema-registry:8081`
- Default parallelism is set to 2

For internal applications (from other Docker containers):

## 🛑 Stopping the Services

To stop the services:

```bash
# Stop Flink
docker-compose -f docker-compose.flink.yml down

# Stop Kafka
docker-compose -f docker-compose.kafka.yml down
```

## 📝 Notes

- This is a development setup and not recommended for production use
- All services are connected through the `easymlops_network` Docker network
- Data persistence is not configured - all data will be lost when containers are removed

## 🔍 Troubleshooting

1. If services fail to start, check:

   - Docker has enough resources allocated
   - No port conflicts on your machine
   - Network `easymlops_network` exists

2. If Kafka is not accessible:

   - Ensure Zookeeper is healthy
   - Check Kafka broker logs
   - Verify network connectivity

3. If Flink jobs fail:
   - Check JobManager logs
   - Verify TaskManager registration
   - Ensure enough resources are available
