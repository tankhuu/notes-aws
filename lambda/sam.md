# Note SAM

```
# Initiation
sam init --runtime python3.8

# Build
cd sam-app
sam build

# Test function locally
sam local invoke "HelloWorldFunction" -e events/event.json
sam local start-api
curl http://127.0.0.1:3000/hello

# Package application
sam package --output-template packaged.yaml --s3-bucket aws-devops-course-stephane --region us-east-2 --profile aws-devops

# Deploy application
sam deploy --template-file packaged.yaml --capabilities CAPABILITY_IAM --stack-name aws-sam-getting-started --region us-east-2 --profile aws-devops

```

## Sam & CodeDeploy

[Gradual Code Deployment](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/automating-updates-to-serverless-apps.html)

If you use AWS SAM to create your serverless application, it comes built-in with CodeDeploy to provide gradual Lambda deployments. With just a few lines of configuration, AWS SAM does the following for you:

- Deploys new versions of your Lambda function, and automatically creates aliases that point to the new version.
- Gradually shifts customer traffic to the new version until you're satisfied that it's working as expected, or you roll back the update.
- Defines pre-traffic and post-traffic test functions to verify that the newly deployed code is configured correctly and your application operates as expected.
- Rolls back the deployment if CloudWatch alarms are triggered.
