# Steps are used to enable CloudWatch Agent on Sending Metrics & Logs to CloudWatch

## 1. Create Agent Instance Role

This role will be attached to EC2 Instances which are installing CloudWatch Agent for sending Metrics & Logs
It must have Policy: _CloudWatchAgentServerPolicy_

## 2. [Optional] Create Admin Instance Role

This role will be attached to EC2 Instance which will be used to send the CloudWatch Agent Configuration into SSM Parameter Store, this Role should be remove from EC2 Instance after used, cause It's very important and make change on the configuration of CloudWatch Agent Configuration.
It must have Policy: _CloudWatchAgentAdminPolicy_

## 3. Install CloudWatch Agent in EC2 Instance with SSM Run Command

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

## 4. Create the CloudWatch Agent Configuration in SSM Parameter Store

We create configuration from the EC2 Instance that has Admin Instance Role above, or create the parameter in SSM Parameter Store directly with the Name of param has prefix: _AmazonCloudWatch-_, value of it can be reference from file `cw_agent_configuration.json`

## 5. Start CloudWatch Agent in EC2 Instance with SSM Run Command

- Go `SSM` -> `Run Command`
- Document: `AmazonCloudWatch-ManageAgent`
- Targets: choose EC2 Instances
- Action: `configure`
- Optional Configuration Source: `ssm`
- Optional Configuration Location: `the name which has prefix _AmazonCloudWatch-_ that we created above`.
- Optional Restart: `yes`
- `Run`
