# Calling downstream functions synchronously

## Invocation options

If you want invoke downstream functions from your upstream function synchronously, you basically have two options:

* HTTP client / REST client via AWS API Gateway
* AWS SDK Lambda client bypassing the AWS API Gateway

> TODO: to be continued...

!!! danger "Avoid calling other functions synchronously" 
    Calling other functions from your function is considered to be an anti-pattern, since it introduces strong coupling
    between functions and unnecessary costs. Prefer an asynchronous communication between lambdas using events and destinations.

@see [Anti-patterns in Lambda-based applications](https://docs.aws.amazon.com/lambda/latest/operatorguide/anti-patterns.html)

@see [Lambda functions calling Lambda functions](https://docs.aws.amazon.com/lambda/latest/operatorguide/functions-calling-functions.html)