Zero Downtime(RTO = 0)
Stack: Route53 -> cloudFront -> ALB
- DNS-Based Failover: Solutions using route53 failover policies rely on DNS updates, which incur Time-to-live (TTL) delay (even with TTL = 0, client may cache DNS Response). This typically result in seconds to minute to delay, failing the zero-seconde RTO
- CloudFront origin Failover: CF can switch origins within the same distribution instantly based on HTTP status codes, avoiding DNS delays and achieving near-instance failover

How do it:
- Create an origin group with the primary and seconde ALB instance as the origin. Configures failover on HTTP5xx status code. Updates the distribution's default behavior to use the origin group