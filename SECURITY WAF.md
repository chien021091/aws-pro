# Defenition
- WAF (web application firewall): protect web application by filtering hhtp/https trafic at the application layer (layer 7)
- It is typically associated like ALB, Cloudfront, API GW
- WAF rules can block IPs, (web trafic), not all VPC trafic (database trafic, non-http protocol)

# Log
- Can put log to cloudwatch, s3, kinesis firehorse
- Must have prefix aws-waf-logs-
- for s3, support encryption (SSE-S3), SSE-KMS, not support KMS keys that are managed by aws