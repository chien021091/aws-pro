- provides a landing zone to manage multi-account environments, applying guadrails (preventive, detective control) to all accounts

## AFC
- Account factory automates accoung provisioning with pre-configured guadrails
- AFC (Account Factory Customization) bluesprints allow customization of new accounts during provisioning, embedding the baseline configuration (via Cloudformation template) directly into the account creation process
- Account Factory provisions accounts with Control Tower guadrails (requirement) automatically applied, ensuring compliance with organization polices

- Baseline configuration: 
    - May include IAM Role, VPC configurations, logging setups, or other standard resources

- Delegate Administration
    - AWS Organization allows delegating certains actions to an another account
    - Delegate Administration is typically used for service like Control tower, catalog


Provide guardrail (proactive and detective control) for enforce policies
- Proactive in aws control tower prevent non-compliance actions before the occur
- Detective in aws control tower indentity non-compliance after the are created


Step AFC
- Create IAM role has AWSControlTowerBlueprintAccess police.Configure the role with a trust policy that allows AWSControlTowerAdmin role in the management account to assume the role. Attach AWSServiceCatalogAdminFullAccess role to the AWSControlTowerBlueprintAccess role
- Create a CloudFormation template that contains the resources for each customization
- Create a service Catalog product for each Cloudformation template




Customization for Control Tower (CFCT):
- An aws-supported solution to extend Control Tower by automating custom resources deployment and SCPs
- Use a CodeCommit repository to store cloudformation templates and SCP json document
- Integrates with Controle Tower's lifecycle event to deploy customization when new account are created

## Note CFCT
- user cloudformation + SCPs deploy via CI/CD pipelines (code build, code pipeline, CodeCommit, S3)
- require manifest.yaml in code commit repository
- can update customization to existing account and new account