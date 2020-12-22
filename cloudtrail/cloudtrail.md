# AWS CloudTrail

## Validate Logs

Checking the integrity of the log files
`aws cloudtrail validate-logs start-time 2020-11-21T00:00:00Z --trail-arn arn:aws:cloudtrail:us-east-2:11111111:trail/my-trail-name --verbose --profile aws-devops --region us-east-2`

## Cross Account Logging

[Document](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-receive-logs-from-multiple-accounts.html)

Edit the Bucket Policy to allow the other Account to write into this Bucket

[CloudTrail Sharing Logs](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-sharing-logs.html)
