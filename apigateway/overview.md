# AWS API Gateway

Protocol:

- REST
- Websocket

Ways to create REST API:

- New API
- Clone from existing API
- Import from Swagger or OpenAPI 3
- Example API

Endpoint Type:

- Regional
- Edge optimized
- Private: are only accessible through VPC endpoints for API Gateway

## Resources

- Path
- Method: in every path
  - ANY
  - DELETE
  - GET
  - POST
  - PUT
  - PATCH
  - HEAD
  - OPTIONS
- Method Integration:
  - Lambda Fuction
  - HTTP
  - Mock
  - AWS Service
  - VPC Link

## Mapping

In Resources -> Method - Integration Request / Repsonse -> Mapping Templates
Integration Request: We can use it to allow API Gateway do some transformation on the contents of the payload before passing it to the lambda function or other integration types. And viseversa on Integration Response we can modify data before sending response to client.

## Deployment Stages

- Whenever we make change on the API Gateway, they will not effective until we did a deployment, then those changes will be deployed to many Stages (as many as you want). Ex. dev | stage | prod
- Each stage has its own configuration parameters
- Each stages can be rolled back as a history of the deployment is kept

## Canary Deployment

Stages -> (prod) -> Canary -> Define the percent of requests on Canary and prod(this is a stage)

Then we can go -> change API -> Deploy to prod (now with Canary Enabled) -> then after we deploy, 10% of requests will go to canary (latest version) and the rest will go to the prod (current working version)
We can change the percent of it for making sure that new version is working ok, then when we are happy with the new version, we can click `promote canary` then the canary will become a main stage and the new version is deployed to prod 100%

-> Up until now, we have 2 ways to do canary deployment for API Gateway (when Integration type is Lambda Function). The first way is using Lambda function Alias for applying canary deployment on Lambda function, and the 2nd way is the above one.

## Throttles

[API Gateway Request Throttling](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-request-throttling.html)

We have throttling in many levels:

- Account level: limits steady-state request rate to 10,000 requests per second (rps).
- API Gateway level
- Usage plan level
- Lambda function level (if Integration Type of API Gateway is Lambda function) -> At the same time, 1,000 lambda functions can be executed concurrently.

Define Usage Plan -> Client A -> Enable Throttling on Rate (rps) and Burst (rps)
And even use this Usage Plan with the API Key to limits the users on throttling associated with a specific API Key.

## Fronting Step Functions

Use API Gateway to expose the state machine of Step Functions to external as an API

API Gateway -> Resources -> Create Resource -> Method -> Integration Type: AWS Service (Many AWS Services can be integrated with API Gateway here) -> Step Functions -> HTTP Method
