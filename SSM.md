SSM supports EC2 instance, on-premises server, IoT Greengrass devices as managed instance
- Managed instance
    - Resources (EC2, on-premise server, IoT devices) must have the SSM Agent installed and be registered with SSM
    - EC2 instance typically use an IAM instance profile for SSM access
    - On-premises service and IoT device require managed-instance activation or specific IAM role