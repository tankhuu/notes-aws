# Kinesis

- Manageed alternative to Apache Kafka with realtime streaming capability.
- Great for application logs, metrics, IoT, clickstreams
- Great for realtime big-data
- Great for streaming process frameworks (Spark, NiFi, ...)
- Data is replicated to 3 AZ automatically

Kinesis Streams: low latency streaming ingest at scale
Kinesis Analytics: perform realtime analytics on streams using SQL
Kinesis Firehose: load stream into S3, Readshift, Elasticsearch, ...

Producers:

- SDK Kinesis Producer Library (KPL)
- Kinesis Agent
- Kinesis Data Streams
- CloudWatch Logs & Events
- IoT rules actions

Kinesis Firehose

AmazonS3
RedShift
Elasticsearch
Splunk

## Kinesis Data Streams vs Firehose

Streams:

- Going to write custom code (producer / consumer)
- Real time (~200ms latency for classic)
- Must manage scaling (sharding splitting / merging)
- Data Storage for 1 to 7 days, replay capability, multi consumers
- Use with Lambda to insert data in real-time to Elasticsearch

Firehose:

- Fully managed, send to S3, Splunk, RedShift, Elasticsearch
- Serverless data transformations with Lambda
- Near real time (lowest buffer time is 1 minute)
- Automated Scaling
- No data storage
