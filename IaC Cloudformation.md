# Context:
- Update template error, Cloudformation want to rollback
- when rollback, error: UPDATE_ROLLBACK_FAILED
## Requirement:
How to force rollback successfully?

## Resolution
- Issue a ContinueUpdateRollback Command
Use the aws cloudformation console or CLI to issue a ContinueUpdateRollback command, instructing CloudFormation to retry the rollback
- Manually Adjust Resources to Match Stack Expectations
Manually modify or delete resources that are causing the rollback faillure to align with cloudformation's state expected

## Attention:
- Cloudformation cannot accept new update requests until the rollback is resolved 
- Cannot update the stack using the original template when UPDATE_ROLLBACK_FAILED



# policy
- DeletionPolicy in CloudFormation supports values like: DELETE, RATAIN, SNAPSHOT (not EMPTY)
- Cloudformation emit some event during stack operation like: CREATE_COMPLETE, UPDATE_COMPLETE, UPDATE_FAILED

# NestedStack
- Allows a parent stack to reference child stack, enabling modular templates with defined dependencies. The dependsOn attribute on resource dependency in parent stack ensure the template deployed in the correct order

# Cloudformation custom resource
- allow Cloudformation to manage resources not natively supported by including a lambda function to handle: create, update, delete operation
    - Cloudformation invodes the lambda function, passing a request(create, update, delete) and a pre-signed S3 url in the payload event data
    - lambda perform the desired action
    - lambda must send a response contain the pre-signed url , indicating success or failure
    - Cloudformation wait for this reponse to change the stack status
- CloudFormation does not support drift detection for custom resource