# Anatomy of a SAM project

Although creating new projects with SAM is pretty easy by just running `sam init`, it doesn't hurt to understand
what kind of files SAM is actually creating for you.

Since the structure of a SAM project depends on the underlying programming language and the deployment packaging option
you use, we will start with a language-specific view before discussing the commonalities.

## Java

This is the structure of a SAML project based on Java 11:

````text
sam-hello-java                         <-- project root folder
├───.aws-sam                           <-- temporary build folder (do not touch!)
│   └───build
├───events
│   └───event.json                     <-- test event
├───HelloWorldFunction                 <-- Maven project folder
│   └───src
│       ├───main
│       │   └───java
│       │       └───helloworld
│       │           └───App.java       <-- lambda function code
│       └───test
│           └───java
│               └───helloworld
│       │           └───AppTest.java   <-- lambda function test code
│       ├───Dockerfile                 <-- multi-mode Docker file
│       └───pom.xml                    <-- Maven POM file to build lambda function
├───README.md                          <-- README with build and deploy instructions
├───samconfig.toml                     <-- SAM configuration file
└───template.yaml                      <-- SAM template file
````

## Go

This is the structure of a SAML project based on Go:

````text
sam-hello-go                           <-- project root folder
├───.aws-sam                           <-- temporary build folder (do not touch!)
│   └───build
├───hello-world                        <-- Maven project folder
│   ├───Dockerfile                     <-- multi-mode Docker file
│   ├───go-mod
│   │   └───go.sum
│   ├───main.go                        <-- lambda function code
│   ├───main_test.go                   <-- lambda function test code
├───README.md                          <-- README with build and deploy instructions
├───samconfig.toml                     <-- SAM configuration file
└───template.yaml                      <-- SAM template file
````

## Common files

Although the structure and the contents of the project directories differ from language to language they contain some
files which are common to all SAM projects.

### SAM configuration file

The default file name of the SAM configuration file is `samconfig.toml`, the configuration file is located at the 
project's root directory.

A SAM project newly created with `sam init` does not contain a configuration file. 
It's created when you run `sam deploy --guided` for the first time.

#### Content

Contents of a simple configuration file:

````toml
version = 0.1
[default]
[default.deploy]
[default.deploy.parameters]
stack_name = "sam-hello-go"
s3_bucket = "aws-sam-cli-managed-default-samclisourcebucket-19afhlgy1ipa9"
s3_prefix = "sam-hello-go"
region = "eu-west-1"
confirm_changeset = true
capabilities = "CAPABILITY_IAM"
image_repositories = ["HelloWorldFunction=928593304691.dkr.ecr.eu-west-1.amazonaws.com/samhellogoc0b96f84/helloworldfunction19d43fc4repo"]
````

* `[default]`: refers to a specific environment
* `[default.deploy]`: begins the configuration section of the SAML `deploy` command
* `[default.deploy.parameters]`: lists values for command line parameters to be passed to the SAML `deploy` command

@see [AWS SAM CLI configuration file](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-config.html)

!!! tip "Use custom SAM configuration files in all your SAM projects"
    Using the guided deployment feature of SAM is not a good idea, if you have many teams working with AWS Lambda
    for the same solution / product, since you will end up with multiple ECR repositories and S3 buckets
    you will have to consolidate later. That being said, it's always a good idea to build your own custom
    configuration files which refer to well-defined and well-configured shared ECR repos and S3 buckets provisioned
    before running SAM deployments.

#### Rules

Here are some rules for SAM configuration files:

* SAM configuration file use TOML tables to group configuration parameters
* The default environment is `default` which can be overridden using the `--config-env` command line parameter
* For commands, the format of the TOML table header is `[${environment}.${command}.parameters]`
* For sub commands, the format of the TOML table header is `[${environment}.${command}_${subcommand}.parameters]`.
  Hyphens (`-`) in command names must be replaced with underscores (`_`).
* The special command name `global` can be used to define global parameters shared by all commands.

#### Supported parameters

Supported deployment parameters are:

| Parameter name          | Description                                                                                                                                               | Matches command line argument               |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------|
| `stack_name`            | Name of the underlying CloudFormation stack                                                                                                               | --stack-name                                |
| `resolve_s3`            | Controls if S3 buckets should be resolved (i.e. prompted for) (`true`) or simply being taken from the configuration file (`false`)                        | --resolve-s3                         |
| `s3_bucket`             | Name of the S3 bucket to store CloudFormation templates and ZIP archives (when using ZIP-based deployment packages)                                       | --s3-bucket                                 |
| `s3_prefix`             | Name prefix of all files stored in the S3 bucket mentioned above                                                                                          | --s3-prefix                                 |
| `resolve_image-repos` | Controls if ECR repos should be resolved (i.e. prompted for) (`true`) or simply being taken from the configuration file (`false`)                         | --resolve-s3                         |
| `image_repository`      | URI of an ECR repo this command should upload the container images to                                                                                     | --image-repository                          |
| `image_repositories`    | Maps a specific function to the name of an ECR repository containing the container deployment package                                                     | --image-repositories                        |
| `region`                | Name of the AWS region to deploy to                                                                                                                       | --region                                    | 
| `capabilities`          | Defines a list of capabilities that the CloudFormation stack must have before it is actually executed (like permission to create new IAM users and roles) | --capabilities                              |
| `signing_profiles`      | Code sign configuration parameters to sign uploaded deployment packages                                                                                   | --signing-profiles                          |
| `confirm_changeset`     | Controls if changes to the underlying CloudFormation stack need to be approved before applying them                                                       | --confirm-changeset, --no-confirm-changeset |
| `disable_rollback`      | Controls if the state of previously provisioned resources should be preserved when an operation fails                                                     | --disable-rollback, --no-disable-rollback   |
| `parameter_overrides`   | List of parameter overrides to be passed to the underlying CloudFormation templates                                                                       | --parameter-overrides                       |
| `tags`                  | List of tags which are to be propagated to all created AWS resources                                                                                      | --tags                                      |

!!! info "Configuration of ECR repos since SAM version 1.82.0" 
    During my upgrade to the latest SAM version in 2023, the configuration of the ECR repo changed: the only way to make an
    unguided deployment work, was to specify `resolve_image_repos = false` and a function-image-mapping with `image_repositories = ["$functionName=$ecrRepoUrl"]`.

### SAM template file

The SAM template file is located in the project root directory and is named `template.yaml` by default.

Since SAM still uses CloudFormation to deploy functions, the SAM template file closely follows the format of a 
[CloudFormation template file](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html).

Contents of a simple SAM template file:

````yaml
AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31
Description: >
  sam-hello-go
  
  Sample SAM Template for sam-hello-go

# More info about Globals: https://github.com/awslabs/serverless-application-model/blob/master/docs/globals.rst
Globals:
  Function:
    Timeout: 5

Resources:
  HelloWorldFunction:
    Type: AWS::Serverless::Function # More info about Function Resource: https://github.com/awslabs/serverless-application-model/blob/master/versions/2016-10-31.md#awsserverlessfunction
    Properties:
      PackageType: Image
      Architectures:
        - x86_64
      Events:
        CatchAll:
          Type: Api # More info about API Event Source: https://github.com/awslabs/serverless-application-model/blob/master/versions/2016-10-31.md#api
          Properties:
            Path: /hello
            Method: GET
      Environment: # More info about Env Vars: https://github.com/awslabs/serverless-application-model/blob/master/versions/2016-10-31.md#environment-object
        Variables:
          PARAM1: VALUE
    Metadata:
      DockerTag: go1.x-v1
      DockerContext: ./hello-world
      Dockerfile: Dockerfile

Outputs:
  # ServerlessRestApi is an implicit API created out of Events key under Serverless::Function
  # Find out more about other implicit resources you can reference within SAM
  # https://github.com/awslabs/serverless-application-model/blob/master/docs/internals/generated_resources.rst#api
  HelloWorldAPI:
    Description: "API Gateway endpoint URL for Prod environment for First Function"
    Value: !Sub "https://${ServerlessRestApi}.execute-api.${AWS::Region}.amazonaws.com/Prod/hello/"
  HelloWorldFunction:
    Description: "First Lambda Function ARN"
    Value: !GetAtt HelloWorldFunction.Arn
  HelloWorldFunctionIamRole:
    Description: "Implicit IAM Role created for Hello World function"
    Value: !GetAtt HelloWorldFunctionRole.Arn
````

* The `Globals` section defines some common configurations for all resources.
* The `Resources` section defines a single resource of type the function `AWS::Serverless::Function` which is actually
a combination of several AWS resources including the lambda function itself plus all required sub-resources of an
API gateway. If the function depends on more AWS resources (like DynamoDB tables or other AWS-managed databases), 
this section should define them as well.
* The `Output` section defines all output values of this template will be display on the console after each successful deployment
