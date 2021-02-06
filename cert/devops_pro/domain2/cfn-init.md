# Note for CFN-Init

- AWS::CloudFormation:Init must be in the Metadata of a resource.
- Make complex EC2 configurations readable
- EC2 instance will query the CloudFormation service to get init data.
- Logs go to `/var/log/cfn-init.log` | `/var/log/cfn-init-cmd.log` | `/var/log/cfn-init-output.log`

## UserData

```
  UserData:
    Fn::Base64:
      !Sub |
        #!/bin/bash -xe
        # Get the latest configuration package
        yum update -y aws-cfn-bootstrap
        # Start cfn-init
        /opt/aws/bin/cfn-init -s ${AWS::StackId} -r MyInstance --region ${AWS::Region} || error_exit 'Failed to run cfn-init'
Metadata:
  Comment: Install a simple Apache HTTP page
  AWS::CloudFormation::Init:
    config:
      packages:
        yum:
          httpd: []
      files:
        "/var/www/html/index.html":
          content: |
           <h1>Hello world from EC2 Instance!</h1>
           <p>This was created using cfn-init</p>
          mode: '000644'
      commands:
        hello:
          command: "echo 'Hello World!'"
      services:
        sysvinit:
          httpd:
            enabled: 'true'
            ensureRunning: 'true'
```

## CFN Signal

- Help to tell CloudFormation that the EC2 instance got properly configured after a cfn-init
- Run cfn-signal right after cfn-init
- Tell CloudFormation service to keep on going or fail

```
  UserData:
    Fn::Base64:
      !Sub |
        #!/bin/bash -xe
        # Get the latest configuration package
        yum update -y aws-cfn-bootstrap
        # Start cfn-init
        /opt/aws/bin/cfn-init -s ${AWS::StackId} -r MyInstance --region ${AWS::Region} || error_exit 'Failed to run cfn-init'
        # Start cfn-signal to the wait condition
        /opt/aws/bin/cfn-signal -e $? --stack ${AWS::StackId} --resource SampleWaitCondition --region ${AWS::Region}

```

## CFN Wait Conditions

- Block the template until it receives a signal from cfn-signal
- We attach a CreationPolicy (also work on EC2, ASG)

```
  SampleWaitCondition:
    CreationPolicy:
      ResourceSignal:
        Timeout: PT1M
    Type: AWS::CloudFormation::WaitCondition
```

## Failures Troubleshooting

> Wait Condition didn't receive the Required Number of Signals from an Amazon EC2 Instance

- Ensure AMI has helper scripts installed.
- Verify the cfn-int & cfn-signal command was successfully run on the instances => Check from logs.
- Verify that the instance has a connection to the Internet.
