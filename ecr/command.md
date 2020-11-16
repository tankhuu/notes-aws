# Commands for ECR

## Variables

```
account_id=696952606624
region=us-east-2
profile=tankhuu-athena
ecr=${account_id}.dkr.ecr.${region}.amazonaws.com
ecr_repo=${ecr}/geoserver
```

## Login

`AWS_PROFILE=${profile} aws ecr get-login-password --region $region | docker login --username AWS --password-stdin $ecr`

## Pull Image

`docker pull ${ecr_repo}:latest`

## Tag Image

`docker tag geonode/geoserver:latest ${ecr_repo}:latest`

## Push Image to ECR

`docker push ${ecr_repo}:latest`
