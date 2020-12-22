# EC2 Instance Compliance

- AWS Config
  - ensure instance has proper AWS configuration (not open SSH port, make sure that it uses c4.large, ...)
  - audit and compliance over time
- Inspector
  - Security Vulnerabilities scan from within the OS using the Inspector agent
  - Outside network scanning (no need for the agent)
- System Manager
  - Run automation, patches, commands, inventory at scale.
- Service Catalog
  - Restrict how EC2 instances can be launched to minimize configurations
  - Helpful to onboard beginer AWS users. (Very good for the beginer in the team)
- Configuration Management
  - SSM, Opsworks, Ansible, Chef, Puppet, User Data
  - Ensure the EC2 instances have proper configuration files.
