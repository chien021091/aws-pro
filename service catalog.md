- AWS service catalog allows administrators to create portfolios of pre-approuved product (comme Cloudformation template) that end users deploy. This ensures users can only deploy resources from approuved templates
- AWS config rules provides continuous monitoring of resource configurations and can detect when resources have drifted from their expected state. Configs rules can be set up to automatically evaluate resources againts desired configurations and report compliance status
- Cloud formation can detect the drift expected state, but which requires manual initiation and doesn't provide the automated monitoring required

## Launch contraint
- Iam role assigned to a Product which allow a user to launch, update or terminate with a minimal permission
- 