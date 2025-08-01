Instant profile
- Bridge for EC2 instance and IAM Role
- When attach to an EC2 instance, the instance can assume the role inside the profile and receive temporaly security credentials via the EC2 metadata service: http://169.254.169.254/latest/meta-data/iam/security-credentials/<role-name>
- Without an instance profile, the EC2 instance wouldn't know how to securely map the role to the instance profile

- Don't have IPv6 elastic IPs

Warm pool
- A warm pool keep pre-initialized instances in the stopped state, reducing scale-out latency
- When the ASG need scale out, it uses instance from warm pool, which start faster than launching instance from scratch
- Instance in the warm pool can run bootstrap script (install dependency) while stopped, futher reducing start time

EC2 Instance Connect
- a way to control short-live SSH key and access control by IAM policy. 
- have ec2-instance-connect agent 
- need public IP, network connectivity

ASG can send notification to SNS with:
- Launch
- Terminate
- Fail to launch
- Fail to terminate

- For taget tracking ASG, not need to create a cloudwatch alarm. it's ASG will magage it (auto create , modify and delete)

## Share AMI from Account A to account B
- In account A, create an AMI encrypted from unencrypted version and specify KMS key in the copy action. Modify the key to give permission to account B for create a grant. Share the encrypted AMI to account B
- In account B, create a grant KMS that delegates a permission to the service-linked role attached to ASG.
-
## Image Builder
- automatic creation AMI or container image
- can be run on schedule
- can publish to multiples regions, accounts
- use RAM (resources access manager) to share across AWS account or organization (can not use parameter store because different region, account, ..)