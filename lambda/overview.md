# Note for Lambda

[VPC Networking for Lambda Function](https://aws.amazon.com/blogs/compute/announcing-improved-vpc-networking-for-aws-lambda-functions/)

## Versions & Aliases

- Versions are immutable (only \$LATEST is mutable)
- Aliases are pointers to Lambda function versions (aliases are mutable)
- We can define a dev | test | prod aliases and have them point at different lambda versions and we can change them at anytime because Aliases are mutable
- Aliases enable Blue / Green deployment by assigning weights to lambda functions
- Aliases enable stable configuration of our event triggers / destinations
- Aliases will have their own ARNs

Versions:

- \$LATEST: mutable
- v1: immutable
- v2: immutable

Aliases:

- dev: \$LATEST (mutable)
- test: v2 (mutable)
- prod: v1 (mutable)

### Define Aliases

View Aliases | Versions:
AWS Console -> Lambda -> Qualifiers -> Versions | Aliases

Create Versions:
Lambda -> Actions -> Publish new version

Create Aliases:
Lambda -> Actions -> Create alias

## Canary & Linear Deployment

Canary & Linear have the same concept on x% traffic on 1 version and y% traffic on another version (blue/green deployment)

The different between them is that. Canary is 2 steps, It has a time when the traffic routing is valid. Like 10% traffic go on version 1 in 10 minutes and after that 100% traffice will go to it.
