# OpsWorks Stacks

Manage infrastructure, configuation and application within one service
Supports Chef & Puppet

- OpsWorks Stacks
  - Layers (OpsWorks | ECS | RDS)
    - Instances

OpsWorks allow to control the scaling type of instances:

- 24/7: Fulltime
- Time-based: start, stop instances base on a specific schedule.
- Load-based: start, stop instances base on the CPU

-> It's not as flexible as AWS AutoScaling

## [Lifecycle Events](https://docs.aws.amazon.com/opsworks/latest/userguide/workingcookbook-events.html)

- Setup
- Configure
- Deploy
- Undeploy
- Shutdown

> Setup, Deploy, Undeploy & Shutdown are instance specific and that can be run individually, but Configure happens on all instances if one of them comes online or goes offline and this could be used to configure all instances all at once and make them aware of each other.

## [Stack Configuration and Deployment Attributes](https://docs.aws.amazon.com/opsworks/latest/userguide/workingcookbook-json.html)

## AutoHealing & CloudWatch Events

If a layer instance fails, It will be auto healed, [Using Auto Healing to replace failed Instances](https://docs.aws.amazon.com/opsworks/latest/userguide/workinginstances-autohealing.html) combine with CloudWatch Event Rules.
