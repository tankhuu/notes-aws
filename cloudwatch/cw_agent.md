# AWS CloudWatch Agent

[Installing the CloudWatch Agent Using AWS Systems Manager](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/installing-cloudwatch-agent-ssm.html)

## [Create IAM Roles and Users for Use with the CloudWatch Agent](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/create-iam-roles-for-cloudwatch-agent.html)

There're 2 roles needed (which we created above):

- One role or user enables CloudWatch agent to be installed on a server and send metrics to CloudWatch.
- One role or user is needed to store your CloudWatch agent configuration in Systems Manager Parameter Store. Parameter Store enables multiple servers to use one CloudWatch agent configuration.

> The ability to write to Parameter Store is a broad and powerful permission. You should use it only when you need it, and it shouldn't be attached to multiple instances in your deployment.

> AWS recently modified the following procedures by using new _CloudWatchAgentServerPolicy_ and _CloudWatchAgentAdminPolicy_ policies created by Amazon, instead of requiring customers to create these policies themselves. To use these policies to write the agent configuration file to Parameter Store and then download it from Parameter Store, your agent configuration file must have a name that starts with _AmazonCloudWatch-_. If you have a CloudWatch agent configuration file with a file name that doesn't start with AmazonCloudWatch-, these policies can't be used to write the file to Parameter Store or to download the file from Parameter Store.

- CloudWatchAgentAdminRole: allow to create the CloudWatch agent configuration file and put it into Systems Manager Parameter Store. This role provides permissions for writing to Parameter Store, in addition to the permissions for reading information from the instance and writing it to CloudWatch.
  - Name: CloudWatchAgentServerRole
  - Trusted entity: AWS service -> EC2
  - CloudWatchAgentServerPolicy
  - AmazonSSMManagedInstanceCore
- CloudWatchAgentServerRole: must attach to each Amazon EC2 instance that runs the CloudWatch agent. This role provides permissions for reading information from the instance and writing it to CloudWatch.
  - Name: CloudWatchAgentAdminRole
  - Trusted entity: AWS service -> EC2
  - CloudWatchAgentAdminPolicy
  - AmazonSSMManagedInstanceCore
  - Note: This role shouldn't be attached to all your servers, and only administrators should use it. After you create the agent configuration file and copy it to Parameter Store, you should detach this role from the instance and use CloudWatchAgentServerRole instead.

## [Installing the CloudWatch Agent on EC2 Instances Using Your Agent Configuration](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/install-CloudWatch-Agent-on-EC2-Instance-fleet.html)

Direct from EC2:
`sudo yum install amazon-cloudwatch-agent`

Use SSM:

- Installing or Updating SSM Agent. [Guide](https://docs.aws.amazon.com/systems-manager/latest/userguide/sysman-install-ssm-agent.html)
- Make sure EC2 Instance has internet access
- Download CloudWatch Agent Package:
  - Go `SSM` -> `Run Command`
  - Document: `AWS-ConfigureAWSPackage`
  - Targets: choose EC2 Instances
  - Action: `Install`
  - Name: `AmazonCloudWatchAgent`
  - Version: `latest`
  - `Run`

## Modify the Common Configuration and Named Profile for CloudWatch Agent

The CloudWatch agent includes a configuration file called common-config.toml. You can use this file optionally specify proxy and Region information.
Linux: `/opt/aws/amazon-cloudwatch-agent/etc/common-config.toml`
Windows: `C:\ProgramData\Amazon\AmazonCloudWatchAgent/common-config.toml`

## Start the CloudWatch Agent

> Use SSM:

- Go `SSM` -> `Run Command`
- Document: `AmazonCloudWatch-ManageAgent`
- Targets: choose EC2 Instances
- Action: `configure`
- Optional Configuration Source: `ssm`
- Optional Configuration Location: `the name of the agent configuration file that you created and saved to Systems Manager Parameter Store`. Detail: [Create the CloudWatch Agent Configuration File](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/create-cloudwatch-agent-configuration-file.html)
- Optional Restart: `yes`
- `Run`

> Use CLI:

Fetch Config:
`sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -s -c ssm:configuration-parameter-store-name`
or
`sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -s -c file:configuration-file-path`

## [Create the CloudWatch Agent Configuration File](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/create-cloudwatch-agent-configuration-file.html)
