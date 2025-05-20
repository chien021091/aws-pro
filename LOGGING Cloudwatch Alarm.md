## Cloudwatch Alarm

Based on metric, not a raw log data.

List Action:
- Notification: we can send a message via SNS Topic
- Trigger a lambda (with version).
- Add Autoscaling for an EC2 instance (add 1 or remove 1 instance), or ECS
- EC2 Action (this action is only available for EC2 Per-Instance Metric)
- System Manager Action: Create OpsItem or Incident


## Cloudwath alarm Concept (Example):
- Datapoint to Alarm
    - Specifies how many datapoints(periods) within the evaluation window must breach the threshold to trigger the alarm
    - Format: M out of N, where N is the total number of periods in the window, and M is the number of periods that must breach
- Evaluation window
    - The total window is 12 hours, and each period is 1 hour, so the alarm evaluates 12 period (N =12)
    - The requirement is to trigger if at least 3 hours breach the threshold (M=3), so the setting should be 3 out of 12.
- Missing data handling
    - Breaching: Assumes missing data would trigger the alarm(treat as below threshold)
    - Not breaching: Assumes missing data would not trigger the alarm (treat as above threshold)


## Cloudwatch Dashboard
- Can use directly from Cloud log Insight
- Cannot import result from Athena
- Can import result from x-ray, but only as timeline or service Map (not pie charte)

Alarm with EC2 Action
Case:
Monitor EC2 health via StatusCheckFailed Metric
- StatusCheckFailed Instance: Indicates with issue instance (OS level problem crash, network, software misconfiguration, resource exhaution)
- StatusCheckFailed system: Indicates issues with the underlying host (power failure, hardware failure, network connectivity)
- Action: reboot, recover, terminate
Recover Action:
- Moving it to new underlying host(resolving power or hardware issue)
- Restarting instance with the same configuration (instance ID, private IP, EBS volumn)
- Preserving all EBS volumn, ensuring no loss data on disk

## Cloudwatch cross-account Observability
- support creating dashboard in the monitoring account for log, trace, metric ....
- Enables monitoring account to search, visualize, and analyze telemetry cross account
- Support to 100000 source account, 

## Cloudwatch log subscription
- get access to real-time feed a log events from cloudwatch log and have it delivered to other service such as: Kinesis stream, kinesis data firehorse, lambda