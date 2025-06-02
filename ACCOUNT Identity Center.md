Attribute based- Access Control (ABAC)
- ABAC use attributes (tags...) to define permission dynamically
- In Iam Identity Center, this is enabled via Attribute for Access Control (AfAC) , which map IdP attribute to AWS Session tags
- Permission can be scopped using the aws:PrincipalTag condition key for match user tag with resource tag


Protocol
- SAML 2.0 Identity Provider: for authentification(SSO)
- SCIM (System for cross-domain Identity Management): for automatic provisioning of users and groups. SCIM enables synchronization of user and group information from the external IdP to AWS.
- SCIM not has the identity source (AD connection), lust set an external IdP