## Kinesiss

Enhanced Fan out
- You can build the consumers that use a feature called enhanced Fan out. This feature lets consumers receive records from a stream with throughput of up to 2MB of data par secend par shard. This throughput is dedicated, which means that consumers that use enhanced fan-out don't have to content with other consumers that are receiving data from the stream.
- Kinesis data streams pushes data records from the stream to consumers that use enhanced fan-out. Therefore, these consumers don't need to poll for data
    - provided throughput dedicated up to 2Mb/s read par shard par consumer
    - By default, the consumer poll data from kinesis stream, but with fanout, kinesis use a push model (http/2) instead of polling, lowing latency from 200 ms



- We can register up to 20 consumers per stream to use enhanced fan-out.
---
ParallelizationFactor
- A feature that allows you to process one shard of a kinesis (dynamo db) data stream with more than one lambda invocation simultaneously.

Compare between kinesis data stream and data firehorse

- Unified Cloudwatch agent can not send direct to AWS firehorse
