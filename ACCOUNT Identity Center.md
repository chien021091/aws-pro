Attribute based- Access Control (ABAC)
- ABAC use attributes (tags...) to define permission dynamically
- In Iam Identity Center, this is enabled via Attribute for Access Control (AfAC) , which map IdP attribute to AWS Session tags
- Permission can be scopped using the aws:PrincipalTag condition key for match user tag with resource tag


Protocol
- SAML 2.0 Identity Provider: for authentification(SSO)
- SCIM (System for cross-domain Identity Management): for automatic provisioning of users and groups. SCIM enables synchronization of user and group information from the external IdP to AWS.
- SCIM not has the identity source (AD connection), lust set an external IdP

identity - > management account , has permission set -> control access to another OU or account
permission set: collection one or more IAM policies

ABAC: permission based on user'attribute store in IAM identity center identity store
user's attribute are mapped from Idp as key-values pair 
By exemple: permission based on title, local, center ....
Usecase: we can change permission by change the user's attribute

SAML: for authentifier 
SCIM: auto provision synchro of user entities from Idp to identity center