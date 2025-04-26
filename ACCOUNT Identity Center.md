Attribute based- Access Control (ABAC)
- ABAC use attributes (tags...) to define permission dynamically
- In Iam Identity Center, this is enabled via Attribute for Access Control (AfAC) , which map IdP attribute to AWS Session tags
- Permission can be scopped using the aws:PrincipalTag condition key for match user tag with resource tag