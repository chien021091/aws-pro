- A deployment group contains individually tagged EC2 instances, EC2 instances in ASG or both
- Deployment that use EC2 instances/on-premises compute platform manage the way in which traffic is directed to instances by using an in-place or blue-green deployment type. During an in-place deployment, CodeDeploy perform a rolling update accross EC2 instances. During a blue-green deployment type, the latest application revision is installed in replaced instances.
- If you use an EC2 instance/ on-premises platform, be aware that blue-green deployments worl only with EC2 instances.
- You can not use canary, linear, or all-at-once configuration for ec2/on-premises compute platform
- You can manage the way in which traffic is shifted to the updated lambda function versions during the deployment by choosing the canary, linear, all-at-one
- You can deploy an ECS containeziration as a task set. You can manage the way in which traffic is shifted to the updated task during the deployment by choosing the canary, linear, all-at-one
- ECS blue-green deployments is supported using both CodeDeploy and cloudformation. For blue-green deployment in cloudFormation, you don't create a CodeDeploy application or deployment group
- Lambda, ECS deployment can not use in-place deployment type.
- In Canary deployment, there are no-option to pause it manually through an approval step

## Deploy iam
- CodeDeploy in EC2 instances need iam role connect to deploy (via IG, NAT, VPC Endpoint), if not -> Skipped