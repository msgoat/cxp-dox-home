# Routing traffic to your functions

## Overview

In AWS you have three options available to route incoming HTTP traffic from the internet to your Lambda functions:

* Application Load Balancer
* API Gateway
* Lambda Function Urls

The following table compares the features of these options:

| Application Load Balancer	                                                                           | Amazon API Gateway | Lambda Function Urls |
|------------------------------------------------------------------------------------------------------| --- | --- |
| Easier to transition existing compute stack where you are already using an Application Load Balancer | Good for building REST APIs and integrating with other services and Lambda functions | |
|  | Export SDK for clients | |
|  | Use throttling and usage plans to control access | |
|  | Maintain multiple versions of an API | |
|  | Canary deployments | |
| Supports authorization via OIDC-capable providers, including Amazon Cognito user pools               | Supports authorization via AWS Identity and Access Management (IAM), Amazon Cognito, and Lambda authorizers | |
| Charged by the hour, based on Load Balancer Capacity Units                                           | Charged based on requests served | |
| May be more cost-effective for a steady stream of traffic                                            | May be more cost-effective for spiky patterns | | 

> TODO: to be continued

## Application Load Balancer


## API Gateway

@see [What is Amazon API Gateway?](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html)

@see [Choosing between HTTP APIs and REST APIs](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api-vs-rest.html)

### HTTP API

@see [Working with HTTP APIs](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api.html)


### REST API

@see [Working with REST APIs](https://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-rest-api.html)

## Lambda Function Urls 

@see [Lambda function URLs](https://docs.aws.amazon.com/lambda/latest/dg/lambda-urls.html)