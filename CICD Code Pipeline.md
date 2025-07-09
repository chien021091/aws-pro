- After pipeline, code pipeline can emit some event in realtime: PipelineExecutionSucceeded, PipelineExecutionFailed
- With code commit, CodePipeline cannot need S3 or Eventbridge for event trigger, it use direct code commit as the source
- Code pipeline use event bright for trigger a pipeline when changes are pushed to source ( like code commit, github ), if no event bright rule is in place ( or misconfigured) code pipeline won,t be trigged


ECS action Provider
- Code pipeline's ecs action deploys a new container image to an ECS service by updating the task definition with the new image tag and redeploying the service
- Requires an imagedefinitions.json file (output from the build stage) the specifies the new image URI

Code Deploy provider
- support blue/green deployement for ecs, allowing advanced deployement strategies (traffic shifting, rollback)
- Requires an appspec.yml file to define the deployement configuration
- More complex for test, eligible for production

## Use Code pipeline deploy a CloudFormation stack from account A to account B
- In account A, create a customer-managed KMS that grant usage permission to account A's Code pipeline  service role and account B. In account B, create a S3 bucket with a bucket policy that grants account A access to the bucket
- In account A, create a service role for the cloud formation stack that includes required permission for the services deployed by the stack. In account B, update the code pipeline configuration to includes the resources associated with the account A
- In accountB, add the AssumeRole permission to account A's Code pipeline service role to allow it to assume the cross-account role in account A