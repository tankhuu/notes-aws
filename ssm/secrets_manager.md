# Secrets Manager

- Storing Secrets
- Capability to force rotation of service every X days
- Automate generation of secrets on rotation (uses Lambda)
- Integration with RDS (MySQL, PostgreSQL, Aurora)
- Secrets are encrypted using KMS
- Mostly mean for RDS Integration

## Secrets Manager VS SSM Parameter Store

- Secrets Manager:
  - Cost more than Parameter Store
  - Automatic rotation of secrets with AWS Lambda
  - Integration with RDS, Redshift, DocumentDB
  - KMS encryption is mandatory
  - Can integrate with CloudFormation
- Parameter Store
  - Simple API
  - No secret rotation
  - KMS encryption is optional
  - Can integrate with CloudFormation
  - Can pull a Secrets Manager secret using the SSM Parameter Store API
