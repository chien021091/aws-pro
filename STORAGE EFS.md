- amazon-efs-utils package is specified to mounting EFS file system and support encrypt transit (using TLS)

## config share EFS between lambda
- Create a VPC peering connection between account A and B. update the EFS file system policy to provide account B with access to mount and write to the EFS file system in account A
- update the lambda execution role with permission to access the VPC and the EFS file system