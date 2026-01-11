# streamlog

A lightweight distributed event streaming platform inspired by Kafka.

---

## Overview

`streamlog` is a distributed event streaming system designed to explore how large-scale systems handle **high-throughput, ordered, and durable event processing**.

The project focuses on understanding:
- Append-only log storage
- Partitioned event streams
- Consumer groups and offsets
- Fault tolerance and replication
- Delivery guarantees and trade-offs

It is inspired by systems such as **Apache Kafka** and **Pulsar**, with an emphasis on **clarity of design over production-scale optimization**.

---

## Problem Statement

Modern backend systems rely heavily on event streams for:
- Asynchronous processing
- Data pipelines
- Decoupled microservices
- Real-time analytics

Building such systems requires careful handling of:
- Ordering guarantees
- Failure recovery
- Consumer coordination
- Throughput vs consistency trade-offs

`streamlog` is a learning-focused implementation that explores these challenges.

---

## Design Goals

- **Durable, append-only event logs**
- **Partitioned streams for scalability**
- **At-least-once message delivery**
- **Fault tolerance under broker failures**
- **Simple, explicit system semantics**

### Non-goals

- Exactly-once delivery semantics
- Geo-replication
- Full Kafka protocol compatibility
- Production-scale performance tuning

---

## Core Concepts

### Topics
A topic represents a named stream of events.

### Partitions
Each topic is split into partitions:
- Each partition is an ordered, append-only log
- Ordering is guaranteed **within a partition**

### Offsets
Each event has a monotonically increasing offset within its partition.

### Consumer Groups
- Consumers coordinate as a group
- Each partition is consumed by only one consumer in a group
- Enables parallel processing

---

## Architecture

```
Producers
   |
   v
+-----------+
|  Broker   |
+-----------+
    |   \
    |    \
    v     v
+-----------+   +-----------+
|  Broker   |   |  Broker   |
+-----------+   +-----------+

Consumers (Groups)
```

- Producers append events to partition leaders
- Followers replicate partition logs
- Consumers read events sequentially by offset

---

## Data Flow

### Produce Path
- Producer sends a batch of events
- Broker appends events to the partition log
- Events are replicated to followers
- Acknowledgment is returned to the producer

### Consume Path
- Consumer fetches events by offset
- Offset is committed after processing
- On failure, consumption resumes from last committed offset

---

## Delivery Semantics

`streamlog` provides **at-least-once delivery**:

- Messages may be delivered more than once
- Messages are never silently lost

This trade-off simplifies failure handling and improves reliability.

---

## Fault Tolerance

The system is designed to handle:

- **Broker failure:** A replica is promoted to leader and continues serving requests.
- **Consumer failure:** Partitions are reassigned to healthy consumers in the group.
- **Network issues:** Producers and consumers retry operations safely.

---

## Observability

`streamlog` exposes metrics to understand system behavior:

- Producer throughput
- Consumer lag
- Replication lag
- Partition leader changes

Metrics are exported in **Prometheus** format.

---

## Repository Layout

```
streamlog/
├── cmd/
│   ├── broker/         # Broker process
│   ├── producer/       # Producer client
│   └── consumer/       # Consumer client
├── internal/
│   ├── log/            # Append-only log storage
│   ├── partition/      # Partition management
│   ├── replication/    # Log replication
│   ├── group/          # Consumer group coordination
│   ├── metadata/       # Cluster metadata
│   └── metrics/        # Prometheus metrics
├── proto/
├── tests/
├── docker-compose.yml
└── README.md
```

---

## Running Locally

### Requirements
- Go 1.21+
- Docker & Docker Compose

### Start brokers and clients

```bash
docker-compose up
```

---

## Project Status

**Status:** Active development

### Implemented
- Append-only partition logs
- Basic producer and consumer APIs
- Offset tracking
- Simple replication
- Metrics export

### Planned
- Consumer group rebalancing
- Improved replication guarantees
- Backpressure handling
- Failure injection testing

---

## Trade-offs

- Prioritizes simplicity and correctness over maximum throughput
- Uses at-least-once delivery to avoid data loss
- Keeps partition leadership model explicit and easy to reason about

These trade-offs are intentional and documented.

---

## Motivation & Learning

This project was built to gain hands-on experience with:

- Event-driven architectures
- Distributed log systems
- Throughput-oriented system design
- Failure handling in data pipelines

---

## Contributing

Issues, discussions, and pull requests are welcome.

The project aims to remain:

- Conceptually clear
- Well-documented
- Educational

---

## License

MIT