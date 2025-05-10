- scan package vulnerability, database CVE, Network reachability (ec2)
- evaluate only ec2 instance, container image, lambda function

AWS Inspector rely on the AWS System Manager (SSM) Agent for agent-based scanning of EC2 instance to collect software inventory and detect vulnabilities. Requirement:
- EC2 instance must have SSM-managed
- IAM permission (AmazonSSMManagedInstanceCore)
- Instance must communicate with service ssm endpoint over HTTPS (port 443)
- Supported OS (linux, macos, window)