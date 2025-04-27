- provides a landing zone to manage multi-account environments, applying guadrails (preventive, detective control) to all accounts
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