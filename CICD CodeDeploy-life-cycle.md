CodeDeploy Deployment Types
CodeDeploy supports three main platforms, each with distinct deployment types and lifecycle events:
- EC2/On-Premises:
    - In-Place Deployment: Updates the application on existing instances.
    - Blue/Green Deployment: Deploys new instances, tests, and shifts traffic.

- Amazon ECS:
    - Blue/Green Deployment: Deploys a new task set, tests, and shifts traffic.
- AWS Lambda:
    - Canary Deployment: Gradually shifts traffic to a new Lambda version.
    - Linear Deployment: Shifts traffic in fixed increments.
    - All-at-Once Deployment: Shifts all traffic immediately.

Each deployment type has a unique set of lifecycle events defined in the AppSpec file (appspec.yml for EC2/On-Premises and ECS, JSON/YAML for Lambda). Lifecycle events specify when scripts or Lambda functions can run during the deployment process.
1. In-Place Deployment (EC2/On-Premises)
In-place deployments update the application on existing EC2 or On-Premises instances. The lifecycle events differ slightly depending on whether a load balancer is used.
Without Load Balancer
Lifecycle Events:
- ApplicationStop:
Purpose: Gracefully stop the running application to prepare for deployment.
Scripts: Run scripts from the previous deployment’s AppSpec file to stop services.
Example: systemctl stop httpd.
Failure: Deployment fails and stops.
Runnable: Yes (scripts on instances).

- DownloadBundle:
Purpose: CodeDeploy agent downloads the application revision (e.g., from S3 or GitHub).
Scripts: Reserved for the CodeDeploy agent; cannot run custom scripts.
Failure: Deployment fails.
Runnable: No.

- BeforeInstall:
Purpose: Perform pre-installation tasks, such as backups or environment checks.
Scripts: Run custom scripts on instances.
Example: Back up configuration files, check disk space.
Failure: Deployment fails.
Runnable: Yes.

- Install:
Purpose: Copy the new application files to the instance.
Scripts: Handled by CodeDeploy agent to copy files to the destination (e.g., /var/www/html).
Example: Deploy a WAR file to a Tomcat directory.
Failure: Deployment fails.
Runnable: No.

- AfterInstall:
Purpose: Perform post-installation tasks, such as setting permissions or configuring files.
Scripts: Run custom scripts on instances.
Example: chmod +x /var/www/html/*.
Failure: Deployment fails.
Runnable: Yes.

- ApplicationStart:
Purpose: Start the application or services stopped during ApplicationStop.
Scripts: Run custom scripts to start services.
Example: systemctl start httpd.
Failure: Deployment fails.
Runnable: Yes.

- ValidateService:
Purpose: Verify the application is running correctly after starting.
Scripts: Run custom scripts for health checks or tests.
Example: Curl a health endpoint (curl http://localhost/health).
Failure: Deployment fails, triggers rollback (if enabled).
Runnable: Yes.

Example : AppSpec Example (without load balancer):
yaml

version: 0.0
os: linux
files:
  - source: /
    destination: /var/www/html
hooks:
  ApplicationStop:
    - location: scripts/stop_server.sh
      timeout: 300
      runas: root
  BeforeInstall:
    - location: scripts/before_install.sh
      timeout: 300
      runas: root
  AfterInstall:
    - location: scripts/after_install.sh
      timeout: 300
      runas: root
  ApplicationStart:
    - location: scripts/start_server.sh
      timeout: 300
      runas: root
  ValidateService:
    - location: scripts/validate_service.sh
      timeout: 300
      runas: root

Rollback: If any runnable event fails (non-zero exit code), CodeDeploy rolls back by re-executing ApplicationStop, Install, and ApplicationStart from the previous AppSpec (if rollback is enabled).

With Load Balancer
When an Application Load Balancer (ALB) or Network Load Balancer (NLB) is used, additional traffic management lifecycle events are included to handle instance deregistration and registration.
Lifecycle Events (all events from without load balancer, plus traffic management):

- ApplicationStop (same as above).
- DownloadBundle (same as above).
- BeforeInstall (same as above).
- Install (same as above).
- AfterInstall (same as above).
- ApplicationStart (same as above).
- ValidateService (same as above).

- BeforeBlockTraffic:
Purpose: Perform tasks before deregistering instances from the load balancer.
Scripts: Run custom scripts on instances.
Example: Log current traffic state, notify monitoring systems.
Failure: Deployment fails.
Runnable: Yes.

- BlockTraffic:
Purpose: Deregister instances from the load balancer to block incoming traffic.
Scripts: Reserved for the CodeDeploy agent; cannot run custom scripts.
Example: CodeDeploy removes instances from ALB target groups.
Failure: Deployment fails.
Runnable: No.

- AfterBlockTraffic:
Purpose: Perform tasks after instances are deregistered from the load balancer.
Scripts: Run custom scripts on instances.
Example: Clean up resources, log deregistration.
Failure: Deployment fails.
Runnable: Yes.

- BeforeAllowTraffic:
Purpose: Perform tasks before registering instances with the load balancer.
Scripts: Run custom scripts on instances.
Example: Verify application health before allowing traffic.
Failure: Deployment fails.
Runnable: Yes.

- AllowTraffic:
Purpose: Register instances with the load balancer to allow incoming traffic.
Scripts: Reserved for the CodeDeploy agent; cannot run custom scripts.
Example: CodeDeploy adds instances to ALB target groups.
Failure: Deployment fails.
Runnable: No.

- AfterAllowTraffic:
Purpose: Perform tasks after instances are registered with the load balancer.
Scripts: Run custom scripts on instances.
Example: Run post-deployment tests, notify monitoring systems.
Failure: Deployment fails, triggers rollback.
Runnable: Yes.

Order:
ApplicationStop → DownloadBundle → BeforeBlockTraffic → BlockTraffic → AfterBlockTraffic → BeforeInstall → Install → AfterInstall → ApplicationStart → ValidateService → BeforeAllowTraffic → AllowTraffic → AfterAllowTraffic.

AppSpec Example (with load balancer):
yaml

version: 0.0
os: linux
files:
  - source: /
    destination: /var/www/html
hooks:
  ApplicationStop:
    - location: scripts/stop_server.sh
      timeout: 300
      runas: root
  BeforeBlockTraffic:
    - location: scripts/before_block_traffic.sh
      timeout: 300
      runas: root
  AfterBlockTraffic:
    - location: scripts/after_block_traffic.sh
      timeout: 300
      runas: root
  BeforeInstall:
    - location: scripts/before_install.sh
      timeout: 300
      runas: root
  AfterInstall:
    - location: scripts/after_install.sh
      timeout: 300
      runas: root
  ApplicationStart:
    - location: scripts/start_server.sh
      timeout: 300
      runas: root
  ValidateService:
    - location: scripts/validate_service.sh
      timeout: 300
      runas: root
  BeforeAllowTraffic:
    - location: scripts/before_allow_traffic.sh
      timeout: 300
      runas: root
  AfterAllowTraffic:
    - location: scripts/after_allow_traffic.sh
      timeout: 300
      runas: root

Rollback: If any runnable event fails, CodeDeploy rolls back by reverting to the previous application version, re-executing relevant events. Traffic management ensures instances are re-registered with the load balancer during rollback.
Notes:
Traffic management events are only included if a load balancer is configured in the deployment group.

These events ensure instances are safely removed from and added to the load balancer, minimizing downtime.

2. Blue/Green Deployment (EC2/On-Premises)
Blue/green deployments for EC2/On-Premises create new instances (green) to replace the existing instances (blue), shift traffic, and terminate the blue instances if successful.
Lifecycle Events:
- BeforeInstall:
Purpose: Perform setup tasks on green instances before installing the application.
Scripts: Run custom scripts on green instances.
Example: Install dependencies, configure environment.
Failure: Deployment fails, green instances are terminated.
Runnable: Yes.

- Install:
Purpose: Copy the new application files to green instances.
Scripts: Handled by CodeDeploy agent.
Example: Deploy files to /var/www/html.
Failure: Deployment fails.
Runnable: No.

- AfterInstall:
Purpose: Perform post-installation tasks on green instances.
Scripts: Run custom scripts.
Example: Set permissions, configure files.
Failure: Deployment fails.
Runnable: Yes.

- ApplicationStart:
Purpose: Start the application on green instances.
Scripts: Run custom scripts.
Example: systemctl start httpd.
Failure: Deployment fails.
Runnable: Yes.

- ValidateService:
Purpose: Verify the application is running on green instances.
Scripts: Run custom scripts.
Example: Test health endpoints.
Failure: Deployment fails, green instances are terminated.
Runnable: Yes.

- BeforeAllowTraffic:
Purpose: Perform tasks before routing traffic to green instances.
Scripts: Run custom scripts on green instances.
Example: Verify instance health.
Failure: Deployment fails.
Runnable: Yes.

- AllowTraffic:
Purpose: Route traffic to green instances (e.g., update load balancer target groups).
Scripts: Reserved for CodeDeploy agent.
Example: Register green instances with ALB.
Failure: Deployment fails.
Runnable: No.

- AfterAllowTraffic:
Purpose: Perform tasks after traffic is routed to green instances.
Scripts: Run custom scripts on green instances.
Example: Log deployment success, run post-deployment tests.
Failure: Deployment fails, traffic reverts to blue instances.
Runnable: Yes.

AppSpec Example:
yaml

version: 0.0
os: linux
files:
  - source: /
    destination: /var/www/html
hooks:
  BeforeInstall:
    - location: scripts/before_install.sh
      timeout: 300
      runas: root
  AfterInstall:
    - location: scripts/after_install.sh
      timeout: 300
      runas: root
  ApplicationStart:
    - location: scripts/start_server.sh
      timeout: 300
      runas: root
  ValidateService:
    - location: scripts/validate_service.sh
      timeout: 300
      runas: root
  BeforeAllowTraffic:
    - location: scripts/before_allow_traffic.sh
      timeout: 300
      runas: root
  AfterAllowTraffic:
    - location: scripts/after_allow_traffic.sh
      timeout: 300
      runas: root

Rollback: If any runnable event fails, CodeDeploy terminates the green instances, reverts traffic to the blue instances, and keeps the blue environment active.
Notes:
Blue/green requires a load balancer (ALB, NLB, or Classic) to manage traffic shifting.

Green instances are launched via Auto Scaling or manual provisioning, replacing blue instances.

3. Blue/Green Deployment (Amazon ECS)
Blue/green deployments for ECS deploy a new task set (green) alongside the existing task set (blue), test the green version, and shift traffic via load balancer listeners.
Lifecycle Events:
- BeforeInstall:
Purpose: Perform setup tasks before creating the green task set.
Scripts: Typically invoke a Lambda function.
Example: Update configuration, validate resources.
Failure: Deployment fails.
Runnable: Yes (Lambda).

- Install:
Purpose: Create the green task set with the new task definition.
Scripts: Handled by CodeDeploy.
Example: Deploy new container images.
Failure: Deployment fails.
Runnable: No.

- AfterInstall:
Purpose: Perform tasks after the green task set is created.
Scripts: Invoke a Lambda function.
Example: Verify task set health.
Failure: Deployment fails, green task set is terminated.
Runnable: Yes.

- BeforeAllowTraffic:
Purpose: Prepare for routing test or production traffic to the green task set.
Scripts: Invoke a Lambda function.
Example: Configure test listeners.
Failure: Deployment fails.
Runnable: Yes.

- AllowTestTraffic:
Purpose: Route test traffic to the green task set via a test listener (e.g., ALB listener on a different port).
Scripts: Handled by CodeDeploy.
Example: Enable test listener for green tasks.
Failure: Deployment fails.
Runnable: No.

- AfterAllowTestTraffic:
Purpose: Run validation tests on the green task set with test traffic.
Scripts: Invoke a Lambda function (e.g., run test scripts, as in your previous question).
Example: Test health endpoints, API functionality.
Failure: Deployment rolls back, green task set is terminated.
Runnable: Yes.

- BeforeAllowTraffic:
Purpose: Prepare for shifting production traffic to the green task set.
Scripts: Invoke a Lambda function.
Example: Final health checks.
Failure: Deployment rolls back.
Runnable: Yes.

- AllowTraffic:
Purpose: Shift production traffic to the green task set by updating the ECS service’s listener.
Scripts: Handled by CodeDeploy.
Example: Update ALB listener to route to green tasks.
Failure: Deployment rolls back.
Runnable: No.

- AfterAllowTraffic:
Purpose: Perform post-deployment tasks after production traffic is shifted.
Scripts: Invoke a Lambda function.
Example: Clean up resources, log metrics.
Failure: Deployment may roll back.
Runnable: Yes.

AppSpec Example:
yaml

version: 0.0
Resources:
  - TargetService:
      Type: AWS::ECS::Service
      Properties:
        TaskDefinition: <TASK_DEFINITION_ARN>
        LoadBalancerInfo:
          ContainerName: "my-app"
          ContainerPort: 80
Hooks:
  AfterAllowTestTraffic:
    - location: RunTestsLambda
      Timeout: 300
      RunAs: null

Rollback: If any runnable event fails (e.g., Lambda error in AfterAllowTestTraffic), CodeDeploy terminates the green task set, keeping the blue task set active.
Notes:
Requires an ALB with production and test listeners.

Hooks typically invoke Lambda functions, as ECS tasks don’t run scripts directly.

4. Lambda Deployment (Canary, Linear, All-at-Once)
Lambda deployments shift traffic from an existing version to a new version using canary, linear, or all-at-once strategies. The lifecycle events are simpler and focus on traffic shifting and validation.
Lifecycle Events:
- BeforeAllowTraffic:
Purpose: Perform pre-deployment tasks before traffic is shifted to the new Lambda version.
Scripts: Invoke a Lambda function (validation hook).
Example: Validate configuration, check dependencies.
Failure: Deployment fails, traffic remains on the old version.
Runnable: Yes (Lambda).

- AllowTraffic:
Purpose: Shift traffic to the new Lambda version (gradually for canary/linear, immediately for all-at-once).
Scripts: Handled by CodeDeploy.
Example: Update alias weights (e.g., 10% to new version for canary).
Failure: Deployment fails, traffic reverts to old version.
Runnable: No.

- AfterAllowTraffic:
Purpose: Perform post-deployment tasks after traffic is shifted.
Scripts: Invoke a Lambda function (validation hook).
Example: Run tests, log metrics.
Failure: Deployment rolls back, traffic reverts to old version.
Runnable: Yes (Lambda).

AppSpec Example (JSON):
json

{
  "version": 0.0,
  "Resources": [
    {
      "myLambdaFunction": {
        "Type": "AWS::Lambda::Function",
        "Properties": {
          "Name": "myLambdaFunction",
          "Alias": "live",
          "CurrentVersion": "1",
          "TargetVersion": "2"
        }
      }
    }
  ],
  "Hooks": [
    {
      "BeforeAllowTraffic": "BeforeAllowTrafficHookLambda"
    },
    {
      "AfterAllowTraffic": "AfterAllowTrafficHookLambda"
    }
  ]
}

Rollback: If a hook fails (Lambda returns an error), CodeDeploy reverts traffic to the old version by updating the Lambda alias.
Notes:
- Canary: Gradually shifts traffic (e.g., 10% increments over time).
- Linear: Shifts traffic in fixed increments (e.g., 10% every minute).
- All-at-Once: Shifts all traffic immediately (no gradual shift).

Hooks are optional and typically used for validation or monitoring.

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

A deployment group defines which instance receive the deployment, identified by tag group
- A tag group specified by one or more tag group (key-value) that an instancr must match to be included in a deployment
- Multiple tag groups are evaluated with OR logic: an instance is match if it match any tag group
- With in single tag group, multiples tags are evaluated by AND logic: an instance is match all tag in a single group

OneAtTime Deploy configuration
- The OneAtTime settings applies to the deployment pace for green fleet (deployment application to one instance at a time)
- for derigistering the original (blue fleet), CodeDeploy doesn't use OneAtTime to control termination timing, instead, termination occurs AllAtOne

Enable rollback
- Can link CodeDeploy rollback to cloudwatch Alarm (EC2), so if the alarm triggered, code deploy will rollback to previous version
- Use cloudwatch alarm, you can monitor metric for the ec2 instance, ASG group being managed by codeDeploy and then invode an action if the metric you are tracking crosses a certain threshold for a defined a period time.