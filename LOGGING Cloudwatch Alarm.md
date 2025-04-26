## Cloudwatch Alarm

List Action:
- Notification: we can send a message via SNS Topic
- Trigger a lambda (with version).
- Add Autoscaling for an EC2 instance (add 1 or remove 1 instance), or ECS
- EC2 Action (this action is only available for EC2 Per-Instance Metric)
- System Manager Action: Create OpsItem or Incident






Cloudwath alarm Concept (Example):
- Datapoint to Alarm
    - Specifies how many datapoints(periods) within the evaluation window must breach the threshold to trigger the alarm
    - Format: M out of N, where N is the total number of periods in the window, and M is the number of periods that must breach
- Evaluation window
    - The total window is 12 hours, and each period is 1 hour, so the alarm evaluates 12 period (N =12)
    - The requirement is to trigger if at least 3 hours breach the threshold (M=3), so the setting should be 3 out of 12.
- Missing data handling
    - Breaching: Assumes missing data would trigger the alarm(treat as below threshold)
    - Not breaching: Assumes missing data would not trigger the alarm (treat as above threshold)



Cloudwatch Dashboard
- Can use directly from Cloud log Insight
- Cannot import result from Athena
- Can import result from x-ray, but only as timeline or service Map (not pie charte)