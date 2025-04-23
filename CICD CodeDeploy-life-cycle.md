https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-appspec-file-structure-hooks.html

For information about which lifecycle event hooks are valid for which deployment and rollback types, see Lifecycle event hook availability.

- ApplicationStop – This deployment lifecycle event occurs even before the application revision is downloaded. You can specify scripts for this event to gracefully stop the application or remove currently installed packages in preparation for a deployment. The AppSpec file and scripts used for this deployment lifecycle event are from the previous successfully deployed application revision.

Note
An AppSpec file does not exist on an instance before you deploy to it. For this reason, the ApplicationStop hook does not run the first time you deploy to the instance. You can use the ApplicationStop hook the second time you deploy to an instance.

To determine the location of the last successfully deployed application revision, the CodeDeploy agent looks up the location listed in the deployment-group-id_last_successful_install file. This file is located in:

/opt/codedeploy-agent/deployment-root/deployment-instructions folder on Amazon Linux, Ubuntu Server, and RHEL Amazon EC2 instances.

C:\ProgramData\Amazon\CodeDeploy\deployment-instructions folder on Windows Server Amazon EC2 instances.

To troubleshoot a deployment that fails during the ApplicationStop deployment lifecycle event, see Troubleshooting a failed ApplicationStop, BeforeBlockTraffic, or AfterBlockTraffic deployment lifecycle event.

- DownloadBundle – During this deployment lifecycle event, the CodeDeploy agent copies the application revision files to a temporary location:

/opt/codedeploy-agent/deployment-root/deployment-group-id/deployment-id/deployment-archive folder on Amazon Linux, Ubuntu Server, and RHEL Amazon EC2 instances.

C:\ProgramData\Amazon\CodeDeploy\deployment-group-id\deployment-id\deployment-archive folder on Windows Server Amazon EC2 instances.

This event is reserved for the CodeDeploy agent and cannot be used to run scripts.

To troubleshoot a deployment that fails during the DownloadBundle deployment lifecycle event, see Troubleshooting a failed DownloadBundle deployment lifecycle event with UnknownError: not opened for reading.

- BeforeInstall – You can use this deployment lifecycle event for preinstall tasks, such as decrypting files and creating a backup of the current version.

- Install – During this deployment lifecycle event, the CodeDeploy agent copies the revision files from the temporary location to the final destination folder. This event is reserved for the CodeDeploy agent and cannot be used to run scripts.

- AfterInstall – You can use this deployment lifecycle event for tasks such as configuring your application or changing file permissions.

- ApplicationStart – You typically use this deployment lifecycle event to restart services that were stopped during ApplicationStop.

- ValidateService – This is the last deployment lifecycle event. It is used to verify the deployment was completed successfully.

- BeforeBlockTraffic – You can use this deployment lifecycle event to run tasks on instances before they are deregistered from a load balancer.

To troubleshoot a deployment that fails during the BeforeBlockTraffic deployment lifecycle event, see Troubleshooting a failed ApplicationStop, BeforeBlockTraffic, or AfterBlockTraffic deployment lifecycle event.

- BlockTraffic – During this deployment lifecycle event, internet traffic is blocked from accessing instances that are currently serving traffic. This event is reserved for the CodeDeploy agent and cannot be used to run scripts.

- AfterBlockTraffic – You can use this deployment lifecycle event to run tasks on instances after they are deregistered from their respective load balancer.

To troubleshoot a deployment that fails during the AfterBlockTraffic deployment lifecycle event, see Troubleshooting a failed ApplicationStop, BeforeBlockTraffic, or AfterBlockTraffic deployment lifecycle event.

- BeforeAllowTraffic – You can use this deployment lifecycle event to run tasks on instances before they are registered with a load balancer.

- AllowTraffic – During this deployment lifecycle event, internet traffic is allowed to access instances after a deployment. This event is reserved for the CodeDeploy agent and cannot be used to run scripts.

- AfterAllowTraffic – You can use this deployment lifecycle event to run tasks on instances after they are registered with a load balancer.





Code deploy status: skipped
Cause:
- The CodeDeploy agent cannot communicate with the Deploy service
- The instances are not properly registered or reconized by CodeDeploy
- The instances IAM role lacks permission to interact with codeDeploy or s3
- The application revision is invalid or missing critical files (appsec.yml)

Code deploy provides predefined deployement configuration for ecs:
- CodeDeployDefault.ECSLinear10PercentEvery1Minutes: shift 10% of trafic every minutes (10 min total)
- CodeDeployDefault.ECSCanary10Percent5Minutes: shift 10% to new version, wait 5 mins, shift the remaining 90%  all at once
- CodeDeployDefault.ECSAllAtOnce: shift all trafic at once