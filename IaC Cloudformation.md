Context:
- Update template error, Cloudformation want to rollback
- when rollback, error: UPDATE_ROLLBACK_FAILED
Requirement:
How to force rollback successfully?

Resolution
- Issue a ContinueUpdateRollback Command
Use the aws cloudformation console or CLI to issue a ContinueUpdateRollback command, instructing CloudFormation to retry the rollback
- Manually Adjust Resources to Match Stack Expectations
Manually modify or delete resources that are causing the rollback faillure to align with cloudformation's state expected

Attention:
- Cloudformation cannot accept new update requests until the rollback is resolved 
- Cannot update the stack using the original template when UPDATE_ROLLBACK_FAILED




- DeletionPolicy in CloudFormation supports values like: DELETE, RATAIN, SNAPSHOT (not EMPTY)
- Cloudformation emit some event during stack operation like: CREATE_COMPLETE, UPDATE_COMPLETE, UPDATE_FAILED

