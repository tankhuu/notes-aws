# Parameter Store

- Secure Storage for configuration and secrets
- Optional seemless encrytion using KMS
- Serverless, Scalable, durable, easy SDK
- Version tracking of configurations / secrets
- Configuration management using Path & IAM
- Notifications with Cloudwatch Events
- Integration with CloudFormation

## Hierarchy

- We can also reference a secret from the secret managers by using `/aws/reference/` path or reference AMI by using `/aws/service/` path

- /my-department/
  - my-app/
    - dev/
      - db-url
      - db-password
    - prod/
      - db-url
      - db-password
  - other-app/
- /other-department/
- /aws/reference/secretsmanager/secret_ID_in_Secrets_Manager
- /aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2
