- provides a landing zone to manage multi-account environments, applying guadrails (preventive, detective control) to all accounts
- Account factory automates accoung provisioning with pre-configured guadrails
- AFC (Account Factory Customization) bluesprints allow customization of new accounts during provisioning, embedding the baseline configuration (via Cloudformation template) directly into the account creation process
- Account Factory provisions accounts with Control Tower guadrails (requirement) automatically applied, ensuring compliance with organization polices

- Baseline configuration: 
    - May include IAM Role, VPC configurations, logging setups, or other standard resources