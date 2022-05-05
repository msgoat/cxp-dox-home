# Your first function Hello World

## Common conventions

* We are going to use a common S3 bucket for the upload of the SAM template files.
* We are going to use a common ECR repository for the upload of the container deployment packages.
* We are going to deploy our functions to AWS region `eu-west-1`.

## Step 1: Generate project skeleton with SAM

!!! tip "Recommended SAM project name"
    Use your team name or your msg user ID as a prefix for the SAM project name. The project name itself should contain
    the name of the REST operation the function is supposed to handle.
    Good samples are: `teamy-get-hello` or `miket92-get-hello`.

1. Run `sam init` to create a skeleton to start from in your favourite workspace directory.
2. Enter __1__ on question `Which template source would you like to use?`
3. Enter __1__ on question `Choose an AWS Quick Start application template`
4. Hit __RETURN__ on question ` Use the most popular runtime and package type? (Python and zip) [y/N]`
5. Pick the runtime of your choice from the list (__5__ for Java 11)
6. Enter __2__ on question `What package type would you like to use?`
7. If you picked Java 11 as a runtime, enter __2__ on question `Which dependency manager would you like to use?`
8. Enter a meaningful unique project name on question `Project name [sam-app]`
9. Check if you have a new directory named like your SAM project in your workspace directory.

## Step 2: Update the REST API path in `template.yaml`

Unfortunately SAM generates a SAM template file with path `/hello` which may result in issues when deploying your
first function since all CXP members will use the same path.

1. Open file `template.yaml`
2. Replace `/hello` with `/${sam-project-name}/hello` in YAML element 
`Resources.HelloWorldFunction.Properties.Events.HelloWorld.Properties.Path` (which should be on line 27)

## Step 3: Update the Function resource name in `template.yaml`

Unfortunately SAM generates a non-unique function name `HelloWorldFunction` in the SAM template file 
which will result in issues when deploying your first function.

1. Open file `template.yaml`
2. Replace `HelloWorldFunction` with `${SamProjectName}Function` in YAML element
   `Resources.HelloWorldFunction` (which should be on line 14)
3. Replace `!GetAtt HelloWorldFunction.Arn` with `!GetAtt ${SamProjectName}Function.Arn` in YAML element
   `Outputs.HelloWorldFunction.Value` (which should be on line 43)
4. Replace `!GetAtt HelloWorldFunctionRole.Arn` with `!GetAtt ${SamProjectName}FunctionRole.Arn` in YAML element
   `Outputs.HelloWorldFunction.Value` (which should be on line 43)

## Step 4: Add a SAM configuration file to your project

In order to use a common S3 bucket and a common ECR repository and the same region, you will need to add a
pre-built SAM configuration file to your SAM project.

Here's a template for the SAM configuration file:

````yaml 
version = 0.1
[default]
[default.deploy]
[default.deploy.parameters]
stack_name = "${sam-project-name}"
s3_bucket = "s3-eu-west-1-cxpserverless-dev-lambda"
s3_prefix = "${sam-project-name}"
region = "eu-west-1"
confirm_changeset = true
capabilities = "CAPABILITY_IAM"
image_repository = "005913962162.dkr.ecr.eu-west-1.amazonaws.com/cxpserverless-dev-lambda"
````

!!! danger Replace a placeholders with actual values
    The template for the SAM configuration file contains placeholders. Please remember to replace them
    with your individual values.

1. Create a new file named `samconfig.toml` in your SAM project folder with the contents of the template.
2. Replace all placeholders `${..}` with meaningful values

## Step 5: Build the function

Now you can build your function by running `sam build`.
All build artifacts will be placed into directory `.aws-sam/build`, except the Docker image containing your 
ready-to-deploy function plus all dependencies.

## Step 6: Invoke the function locally

One of the great capabilities of SAM is running your functions locally with `sam local invoke` which should 
produce the following output:s

````shell
$ sam local invoke
Invoking Container created from cxpsamgethellojavafunction:java11-maven-v1
Image was not found.
Removing rapid images for repo cxpsamgethellojavafunction
Building image.................
Skip pulling image and use local one: cxpsamgethellojavafunction:rapid-1.43.0-x86_64.

START RequestId: 0651f947-64ca-44f7-9d26-97f1e970c46b Version: $LATEST
Picked up JAVA_TOOL_OPTIONS: -XX:+TieredCompilation -XX:TieredStopAtLevel=1
END RequestId: 0651f947-64ca-44f7-9d26-97f1e970c46b
REPORT RequestId: 0651f947-64ca-44f7-9d26-97f1e970c46b  Init Duration: 1.11 ms  Duration: 977.88 ms     Billed Duration: 978 ms Memory Size: 128 MB     Max Memory Used: 128 MB
{"statusCode":200,"headers":{"X-Custom-Header":"application/json","Content-Type":"application/json"},"body":"{ \"message\": \"hello world\", \"location\": \"93.215.106.81\" }"} 
````

## Step 7: Deploy the function to AWS

Since everything seem to run smoothly, it's about time to deploy your function to AWS.

1. Just run `sam deploy` to start the deployment process.
2. Confirm with __y__ if your are asked `Deploy this changeset? [y/N]`. 
3. The deployment process 
    1. pushes the container deployment package to the ECR repository.
    2. creates a CloudFormation stack from the SAM template and executes it
    3. which will create the IAM execution role for your function
    4. and will deploy the function to the Lambda environment
    5. and will deploy the REST API to the API gateway

## Step 8: Invoke the function on AWS

1. Now you can invoke your function on AWS by entering its API Gateway endpoint URL in your favourite browser.

> The URL of the API Gateway endpoint is shown as output value `HelloWorldApi` on your console. 

## Step 9: Clean up

1. Finally you can delete the function and all related resources by running `sam delete`.


