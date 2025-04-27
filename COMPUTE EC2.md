Instant profile
- Bridge for EC2 instance and IAM Role
- When attach to an EC2 instance, the instance can assume the role inside the profile and receive temporaly security credentials via the EC2 metadata service: http://169.254.169.254/latest/meta-data/iam/security-credentials/<role-name>
- Without an instance profile, the EC2 instance wouldn't know how to securely map the role to the instance profile

- Don't have IPv6 elastic IPs

Warm pool
- A warm pool keep pre-initialized instances in the stopped state, reducing scale-out latency
- When the ASG need scale out, it uses instance from warm pool, which start faster than launching instance from scratch
- Instance in the warm pool can run bootstrap script (install dependency) while stopped, futher reducing start time