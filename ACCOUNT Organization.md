- SCP does not have allow statement
- SCPs cannot be attached to services - they must be attached to the organization root, OUs, or accounts.
- SCP don't grant permissions, they just filter what's available
- SCP affect all users/roles in an account including the roor user (except for service-linked roles)



- The management account has full control over the organization, including the ability to create an account
- By default, only the management account can perform Organization API actions unless permissions are delegated
- To allow a service (lambda) in a different account (not a management account) to call an Organization APIs, the services (lambda) must assume a role in a management account with the nessesary permissions

- By default, in AWS organization, the FullAWSAccess SCP is typically applied at the organization root by default in Control Tower to allow all action unless explicitly denied by other SCPs.

- OrganizationAccountAccessRole: IAM Role which grant full administrator permissions in the member account to the management account