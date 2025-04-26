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