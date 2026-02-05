# Real-Time Data Streaming — Azure Implementation

**Scope**: Real-time data streaming patterns and implementation using Azure Event Hubs, Azure Databricks, Microsoft Fabric, and Power BI.

**Purpose**: Use this when implementing real-time data pipelines, streaming analytics, and live dashboards. For batch pipeline patterns, see [Data Engineering](engineering.md). For platform-specific architecture, see [Fabric Architecture](../cloud/fabric_architecture.md) and [Databricks Architecture](../cloud/databricks_architecture.md).

## Table of Contents

- [1. Streaming Fundamentals](#1-streaming-fundamentals)
- [2. Azure Streaming Architecture](#2-azure-streaming-architecture)
- [3. Azure Event Hubs](#3-azure-event-hubs)
- [4. Databricks Structured Streaming](#4-databricks-structured-streaming)
- [5. Fabric Real-Time Intelligence](#5-fabric-real-time-intelligence)
- [6. Power BI Real-Time Dashboards](#6-power-bi-real-time-dashboards)
- [7. Stream Processing Patterns](#7-stream-processing-patterns)
- [8. Implementation Patterns](#8-implementation-patterns)
- [9. Reliability & Operations](#9-reliability--operations)
- [10. Best Practices Checklist](#10-best-practices-checklist)

---

## 1. Streaming Fundamentals

### Batch vs Streaming

| Aspect | Batch Processing | Stream Processing |
|--------|-----------------|-------------------|
| **Latency** | Minutes to hours | Milliseconds to seconds |
| **Data volume** | Large datasets | Continuous flow |
| **Processing** | Complete dataset | Event-by-event or micro-batch |
| **Use case** | Historical analysis | Real-time insights, alerts |

### Streaming Delivery Semantics

- **At-most-once**: Events may be lost, no duplicates (lowest latency)
- **At-least-once**: No loss, possible duplicates (common default)
- **Exactly-once**: No loss, no duplicates (highest overhead, use idempotent writes)

### Key Concepts

- **Event time**: When event occurred at source
- **Processing time**: When event is processed
- **Ingestion time**: When event enters streaming system
- **Watermark**: Threshold for handling late-arriving events
- **Window**: Time-based grouping for aggregations (tumbling, sliding, session)
- **Checkpoint**: Saved state for fault tolerance and recovery

---

## 2. Azure Streaming Architecture

### Reference Architecture

```
Sources → Ingestion → Processing → Serving → Consumption
   │          │            │          │           │
   ├─ IoT     ├─ Event     ├─ Databricks│ Delta   ├─ Power BI
   ├─ Apps    │  Hubs      │  Streaming │  Tables │  (Real-time)
   ├─ Logs    ├─ IoT Hub   ├─ Fabric    ├─ SQL    ├─ APIs
   └─ Events  └─ Kafka     │  RTI       └─ Event  └─ Alerts
                           └─ ASA           Hubs
```

### Platform Selection

| Platform | Best For | Latency | Complexity |
|----------|----------|---------|------------|
| **Fabric Real-Time Intelligence** | Unified Fabric stack, low-code | Seconds | Low |
| **Databricks Structured Streaming** | Complex transforms, ML | Seconds-minutes | Medium-High |
| **Azure Stream Analytics** | Simple SQL queries | Seconds | Low |
| **Event Hubs + Functions** | Event routing, triggers | Milliseconds | Medium |

### When to Use Each

- **Fabric RTI**: Fabric-native, KQL queries, OneLake integration, Real-Time Hub
- **Databricks Streaming**: Complex joins, ML inference, Delta Live Tables
- **Stream Analytics**: Simple aggregations, IoT scenarios, quick deployment
- **Event-driven Functions**: Event routing, fan-out, microservices triggers

---

## 3. Azure Event Hubs

### Core Concepts

- **Namespace**: Container for Event Hubs (shared throughput units)
- **Event Hub**: Individual event stream (topic)
- **Partition**: Ordered sequence of events (parallelism unit)
- **Consumer Group**: Independent view of event stream (multiple consumers)
- **Offset**: Position in partition (for replay and recovery)

### Configuration Patterns

- **Partition count**: Start with 4-8; increase for throughput (cannot decrease after creation)
- **Throughput units**: 1 TU = 1 MB/s ingress, 2 MB/s egress
- **Retention**: 1-7 days (standard), up to 90 days (premium)
- **Capture**: Auto-archive to ADLS/Blob in Avro format

### Event Hubs Tiers

| Tier | Use Case | Throughput | Features |
|------|----------|------------|----------|
| **Basic** | Dev/test | 1 TU | Limited |
| **Standard** | Production | 20 TU | Consumer groups, capture |
| **Premium** | Enterprise | Up to 10 PU | Dedicated, private link |
| **Dedicated** | High volume | 20+ CU | Isolated, predictable |

### Producer Patterns

```python
# Python - Send events to Event Hubs
from azure.eventhub import EventHubProducerClient, EventData

producer = EventHubProducerClient.from_connection_string(conn_str, eventhub_name)
batch = producer.create_batch()
batch.add(EventData('{"sensor_id": "s1", "value": 42}'))
producer.send_batch(batch)
```

### Consumer Patterns

- **Event Processor**: Recommended for production (load balancing, checkpointing)
- **Consumer Client**: Simple consumption (single partition, no load balancing)
- **Checkpoint**: Store offsets in Blob Storage for recovery

### Best Practices

- **Partition key**: Use consistent key for ordering (device ID, user ID)
- **Batching**: Batch events for throughput (up to 1MB per batch)
- **Compression**: Compress payloads client-side for large events
- **Schema**: Use schema registry for schema evolution and validation
- **Capture**: Enable capture for backup and replay capability

---

## 4. Databricks Structured Streaming

### Core Concepts

- **Streaming DataFrame**: DataFrame API for streaming data
- **Trigger**: When to process (continuous, micro-batch, once)
- **Checkpoint**: State storage for fault tolerance
- **Watermark**: Late data handling threshold
- **Output mode**: Append, update, complete

### Reading from Event Hubs

```python
# Databricks - Read from Event Hubs
connection_string = dbutils.secrets.get("keyvault", "eventhub-conn")

df = (spark.readStream
    .format("eventhubs")
    .option("eventhubs.connectionString", connection_string)
    .option("eventhubs.consumerGroup", "$Default")
    .option("startingPosition", '{"offset": "-1", "isInclusive": true}')
    .load()
)

# Parse JSON payload
from pyspark.sql.functions import from_json, col
schema = "sensor_id STRING, value DOUBLE, timestamp TIMESTAMP"
parsed = df.select(from_json(col("body").cast("string"), schema).alias("data"))
```

### Writing to Delta Tables

```python
# Write stream to Bronze Delta table
(parsed
    .writeStream
    .format("delta")
    .outputMode("append")
    .option("checkpointLocation", "/checkpoints/sensor_bronze")
    .trigger(processingTime="10 seconds")
    .table("bronze.sensor_events")
)
```

### Trigger Modes

| Trigger | Latency | Use Case |
|---------|---------|----------|
| `processingTime("10 seconds")` | 10+ seconds | Balanced cost/latency |
| `processingTime("1 minute")` | 1+ minute | Cost-optimized |
| `availableNow()` | N/A | One-time catch-up |
| `continuous("1 second")` | ~1 second | Low latency (experimental) |

### Watermarking & Late Data

```python
# Handle late-arriving data with watermark
from pyspark.sql.functions import window

(df
    .withWatermark("event_time", "10 minutes")  # Allow 10 min late
    .groupBy(
        window("event_time", "5 minutes"),       # 5-min tumbling window
        "sensor_id"
    )
    .agg({"value": "avg"})
)
```

### Delta Live Tables (DLT)

```python
# Delta Live Tables - declarative streaming pipeline
import dlt

@dlt.table(comment="Raw sensor events from Event Hubs")
def bronze_sensors():
    return (spark.readStream
        .format("eventhubs")
        .options(**eventhub_config)
        .load()
    )

@dlt.table(comment="Cleaned sensor data")
@dlt.expect_or_drop("valid_value", "value > 0 AND value < 1000")
def silver_sensors():
    return dlt.read_stream("bronze_sensors").select(...)
```

### Best Practices

- **Checkpoint location**: Use durable storage (ADLS, DBFS)
- **Partition strategy**: Match source partitions for parallelism
- **File sink optimization**: Use `coalesce()` to avoid small files
- **Auto-scaling**: Enable cluster autoscaling for variable load
- **Monitoring**: Use Databricks streaming metrics and Azure Monitor

---

## 5. Fabric Real-Time Intelligence

### Core Components

- **Real-Time Hub**: Unified experience for discovering and managing streams
- **Eventstream**: Visual stream processing with no-code transforms
- **KQL Database**: Time-series optimized storage for streaming data
- **KQL Queryset**: Ad-hoc and scheduled queries on streaming data

### Real-Time Hub Sources

- **Azure Event Hubs**: Primary streaming source
- **Azure IoT Hub**: IoT device telemetry
- **Fabric events**: OneLake file events, Spark job events
- **Custom sources**: Kafka, webhooks, custom connectors

### Eventstream Processing

```
Event Hubs → Eventstream → Transformations → Destinations
                │              │                  │
                ├─ Filter      ├─ Parse JSON      ├─ KQL Database
                ├─ Union       ├─ Add columns     ├─ Lakehouse
                └─ Split       └─ Aggregate       └─ OneLake
```

### KQL Database Patterns

```kusto
// Create table for streaming data
.create table SensorReadings (
    SensorId: string,
    Value: real,
    Timestamp: datetime,
    IngestionTime: datetime
)

// Streaming ingestion policy
.alter table SensorReadings policy streamingingestion enable

// Query recent data
SensorReadings
| where Timestamp > ago(1h)
| summarize AvgValue = avg(Value) by bin(Timestamp, 5m), SensorId
| render timechart
```

### OneLake Integration

- **Delta output**: Write streaming data directly to OneLake Delta tables
- **Medallion pattern**: Stream to Bronze, then batch process to Silver/Gold
- **Shortcuts**: Reference streaming data from other Fabric workloads

### Best Practices

- **Partitioning**: Partition KQL tables by time (datetime column)
- **Retention**: Set retention policies per table (hot vs warm storage)
- **Batching**: Use batching in Eventstream for throughput
- **Caching**: Use materialized views for frequently queried aggregations

---

## 6. Power BI Real-Time Dashboards

### Real-Time Options

| Mode | Latency | Data Source | Use Case |
|------|---------|-------------|----------|
| **Push datasets** | Seconds | API push | Live metrics, IoT |
| **Streaming datasets** | Seconds | API push | Simple gauges, cards |
| **DirectQuery** | Seconds-minutes | SQL source | Real-time from warehouse |
| **Direct Lake** | Near real-time | OneLake Delta | Fabric streaming to BI |

### Push Dataset Pattern

```python
# Push data to Power BI streaming dataset
import requests

powerbi_url = "https://api.powerbi.com/v1.0/.../push"
payload = [{"sensor_id": "s1", "value": 42, "timestamp": "2026-02-05T10:00:00Z"}]

response = requests.post(powerbi_url, json=payload)
```

### Real-Time Dashboard Components

- **Streaming tiles**: Auto-refresh cards, gauges, KPIs
- **Live page refresh**: Auto-refresh every 30 seconds (minimum)
- **Change detection**: Refresh when underlying data changes

### Fabric Integration for Real-Time BI

```
Event Hubs → Fabric RTI → KQL Database → Power BI Report
     │                          │              │
     └─ Eventstream ────────────┘       DirectQuery to KQL
```

### DirectQuery to KQL Database

- **Connection**: Power BI DirectQuery to Fabric KQL Database
- **Refresh**: Real-time queries on streaming data
- **Aggregations**: Use KQL materialized views for performance
- **Filters**: Push filters to KQL for efficient queries

### Best Practices

- **Dashboard refresh**: Set appropriate refresh intervals (balance freshness vs load)
- **Aggregated metrics**: Pre-aggregate in streaming layer for dashboard performance
- **Tile limits**: Limit streaming tiles per page (max 25 recommended)
- **Historical data**: Combine streaming tiles with historical visuals

---

## 7. Stream Processing Patterns

### Windowing

| Window Type | Description | Use Case |
|-------------|-------------|----------|
| **Tumbling** | Fixed, non-overlapping | Hourly counts, daily summaries |
| **Sliding** | Fixed, overlapping | Moving averages |
| **Session** | Activity-based, gap-triggered | User sessions |
| **Hopping** | Fixed size, fixed hop | 5-min windows every 1-min |

### Aggregations

```python
# Databricks - Tumbling window aggregation
(df
    .withWatermark("event_time", "5 minutes")
    .groupBy(
        window("event_time", "1 hour"),  # Tumbling 1-hour window
        "device_id"
    )
    .agg(
        avg("temperature").alias("avg_temp"),
        max("temperature").alias("max_temp"),
        count("*").alias("event_count")
    )
)
```

### Stream-to-Stream Joins

```python
# Join two streams within time window
clicks = spark.readStream.format("delta").table("clicks")
impressions = spark.readStream.format("delta").table("impressions")

(clicks
    .withWatermark("click_time", "10 minutes")
    .join(
        impressions.withWatermark("impression_time", "10 minutes"),
        expr("""
            click_user_id = impression_user_id AND
            click_time BETWEEN impression_time AND impression_time + INTERVAL 5 MINUTES
        """)
    )
)
```

### Stream-to-Static Joins

```python
# Enrich streaming data with dimension table
events = spark.readStream.format("delta").table("events")
products = spark.read.table("dim_products")  # Static dimension

enriched = events.join(products, events.product_id == products.product_id)
```

### Change Data Capture (CDC)

```python
# Read CDC from Delta table (change data feed)
(spark.readStream
    .format("delta")
    .option("readChangeFeed", "true")
    .option("startingVersion", 0)
    .table("source_table")
)
```

---

## 8. Implementation Patterns

### Medallion Streaming Architecture

```
Event Hubs → Bronze (raw) → Silver (cleaned) → Gold (aggregated)
    │            │               │                   │
    └─ Append ───┴─ Dedupe ──────┴─ Aggregate ───────┴─ Serve
```

### Bronze Layer (Raw Streaming)

- **Append-only**: Never modify, only append
- **Minimal processing**: Parse JSON, add ingestion timestamp
- **Partition by time**: Hourly or daily partitions
- **Checkpoint per source**: Separate checkpoints per stream

### Silver Layer (Cleaned Streaming)

- **Deduplication**: Remove duplicates using watermarks
- **Validation**: Apply data quality rules
- **Standardization**: Normalize schemas, data types
- **Enrichment**: Join with reference data

### Gold Layer (Aggregated)

- **Pre-aggregation**: Compute common aggregations
- **Materialized views**: Power BI-ready metrics
- **Star schema updates**: Update dimensions, append facts
- **Time-series optimization**: Partition by time for queries

### Multi-Hop Streaming Pipeline

```python
# Bronze: Raw events
bronze = (spark.readStream.format("eventhubs").load()
    .writeStream.format("delta").table("bronze_events"))

# Silver: Cleaned events (read from Bronze)
silver = (spark.readStream.table("bronze_events")
    .withWatermark("event_time", "10 minutes")
    .dropDuplicates(["event_id"])
    .writeStream.format("delta").table("silver_events"))

# Gold: Aggregated metrics (read from Silver)
gold = (spark.readStream.table("silver_events")
    .groupBy(window("event_time", "1 hour"), "device_id")
    .agg(avg("value").alias("avg_value"))
    .writeStream.format("delta").table("gold_hourly_metrics"))
```

---

## 9. Reliability & Operations

### Fault Tolerance

- **Checkpointing**: Enable checkpoints for exactly-once processing
- **Idempotent writes**: Use Delta MERGE for safe reruns
- **Dead letter queues**: Route failed events for investigation
- **Retry policies**: Configure retries with exponential backoff

### Monitoring Metrics

| Metric | Description | Alert Threshold |
|--------|-------------|-----------------|
| **Input rate** | Events per second | Varies by workload |
| **Processing rate** | Events processed per second | Should match input |
| **Batch duration** | Time to process micro-batch | Should be < trigger interval |
| **Consumer lag** | Unprocessed events | Growing lag = problem |
| **Checkpoint latency** | Time to checkpoint | Should be stable |

### Databricks Monitoring

```python
# Query streaming metrics
query = df.writeStream.format("delta").start()
print(query.lastProgress)  # Latest batch metrics
print(query.recentProgress) # Recent batches
```

### Event Hubs Monitoring

- **Incoming requests**: Monitor ingestion rate
- **Outgoing requests**: Monitor consumption rate
- **Throttled requests**: Indicates capacity issues
- **Consumer lag**: Events behind in consumer group

### Backpressure Handling

- **Autoscale consumers**: Scale Databricks clusters on lag
- **Throttle producers**: Rate limit upstream when downstream saturated
- **Queue buffering**: Use Event Hubs as buffer during spikes
- **Capacity planning**: Size Event Hubs TUs for peak load

### Disaster Recovery

- **Geo-replication**: Event Hubs geo-disaster recovery (paired regions)
- **Checkpoint backup**: Replicate checkpoints to secondary region
- **Multi-region processing**: Active-passive or active-active consumers
- **RTO/RPO targets**: Define and test recovery objectives

---

## 10. Best Practices Checklist

### Architecture

- [ ] Define latency requirements (seconds vs minutes)
- [ ] Choose appropriate platform (Fabric RTI, Databricks, ASA)
- [ ] Design medallion layers (Bronze → Silver → Gold)
- [ ] Plan for late-arriving data (watermarks)
- [ ] Document event schemas and contracts

### Event Hubs

- [ ] Size partitions for throughput (cannot reduce later)
- [ ] Use partition keys for ordering requirements
- [ ] Enable capture for backup/replay
- [ ] Set appropriate retention period
- [ ] Configure consumer groups per application

### Processing

- [ ] Enable checkpointing for fault tolerance
- [ ] Use watermarks for late data handling
- [ ] Implement idempotent writes (Delta MERGE)
- [ ] Optimize trigger intervals for cost/latency balance
- [ ] Handle schema evolution gracefully

### Monitoring

- [ ] Monitor consumer lag and input/output rates
- [ ] Set alerts for processing delays
- [ ] Track checkpoint latency and failures
- [ ] Monitor cluster/capacity utilization
- [ ] Implement dead letter queues for failed events

### Power BI Integration

- [ ] Use DirectQuery to KQL for real-time dashboards
- [ ] Pre-aggregate metrics in streaming layer
- [ ] Set appropriate dashboard refresh intervals
- [ ] Limit streaming tiles per page
- [ ] Combine real-time with historical context

### Security

- [ ] Use managed identity for Event Hubs access
- [ ] Enable private endpoints for Event Hubs
- [ ] Encrypt data in transit and at rest
- [ ] Implement access controls (consumer groups, RBAC)
- [ ] Audit streaming access and operations

---

> **Note**: For batch pipeline patterns, see [Data Engineering](engineering.md). For Fabric-specific patterns, see [Fabric Architecture](../cloud/fabric_architecture.md). For Databricks-specific patterns, see [Databricks Architecture](../cloud/databricks_architecture.md). For Power BI optimization, see [Power BI Optimization](powerbi.md).
