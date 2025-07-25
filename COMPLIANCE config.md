- detect the not compliance, not prevent
- Config rules are not real-time

Conformance pack
- is a collection of AWS config rules and remediation action deployed across multipe account
- the approuved-amis-by-id rule can be included in a conformance pack to check AMI compliance 
- the conformance pack can be configure the aws-StopEC2Instance runbook to stop noncompliant instance

Users creating remdiation actions need iam:PassRole permissions to assign the role

AWS Config use SNS for stream all the notifications and configuration changes. NOT selectively for a given rule (can not use direct SNS for send email in the case compliance)

## Remediation can not invoke direct lambda, pass by aws ssm automation document. we can use ssm to trigger an lambda, or use event bridge for detect non-compliant event and trigger lambda