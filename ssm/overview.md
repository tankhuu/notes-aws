# Overview on Systems Manager

- Manage EC2 & On-primise system at scale
- General Insights about the state of your infrastructure
- Need SSM Agent installed in the systems we control

## Components

- Resource Groups
- Insights:
  - Dashboard
  - Inventory: discover & audit the software installed
  - Compliance
- Parameter Store
- Action:
  - Automation: (shutdown EC2, create AMIs automatically)
  - Run Command
  - Session Manager
  - Patch Manager
  - Maintenance Windows
  - State Manager: define and maintain configuration of OS and applications

## Resource Groups

A way to group resources together.
Types:

- Tag based: group resources by specifying tags that are shared by the resources.
- CloudFormation stack based: Create a resource group based on an existing CloudFormation stack. The group will have the same logical structure as the stack.
