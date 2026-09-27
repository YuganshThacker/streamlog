# streamlog

A distributed event-streaming architecture study inspired by Kafka and Pulsar.

## Current Status

**Scaffold / design study.** The current public repository documents the intended broker, partition, replication, and consumer-group model. The runtime implementation is not complete yet.

## Intended Architecture

Producers → Partition Leader → Replicas → Consumers / Consumer Groups

The design explores ordered append-only logs, offsets, partitioning, replication, at-least-once delivery, and consumer coordination.

## Design Focus

- partition-level ordering
- append-only durable logs
- replication and leader changes
- consumer offsets and groups
- failure recovery
- throughput vs consistency trade-offs

## Next Steps

- implement broker and partition storage
- implement replication
- add consumer-group coordination
- add failure injection and throughput benchmarks

Claims about runtime throughput or fault tolerance are intentionally not made until they are backed by code and tests.
