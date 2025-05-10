- AWS Fargate don't support EFS CSI Driver for EKS (EC2 support)

- EKS Control Plan is managed by AWS and does not run user workload or store image for pod (can not intervenir)

- EKS support Cloudwatch log to capture controle plan, worker node and application log (not S3)