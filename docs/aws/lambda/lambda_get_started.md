# Getting started with AWS Lambda

## Prerequisites

* You will need access to an AWS account.
* The [AWS CLI]() must be installed on your local machine.
* The [AWS SAM CLI]() must be installed on your local machine.
* Installing the AWS Toolkit Plugin for your favourite IDE is highly recommended.

## AWS Serverless Application Model (AWS SAM)

The `AWS Serverless Application Model (AWS SAM)` is an open-source framework which helps you 

* to develop and test AWS Lambdas on your local machine
* to build and deploy AWS Lambdas to AWS accounts

@see [AWS Serverless Applicaton Model](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html)

### HOW-TO install AWS SAM

There's an excellent [installation guide provided by AWS](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html) about the installation of AWS SAM on your local machine.
Since we are using primarily Windows machines, you should follow the installation instructions for Windows.
Of course, you can install AWS SAM on a Window WSL2 Ubuntu as well.

### SAM command cheat sheet

After installing SAM on your local machine, you simply can run SAM commands by entering `sam ${command}`.

Here's a list of the most important SAM commands.

| Command name | Description                                                                                             | 
|--------------|---------------------------------------------------------------------------------------------------------|
| `--help`     | Explains general usage of SAM; can be applied to SAM commands as well                                   |
| `init`       | Initializes a SAM project based on AWS-managed or your own custom templates                             |
| `validate`   | Validates your SAM template file                                                                        |
| `build`      | Builds your function code and tests it                                                                  |
| `local`      | Runs and invokes your function locally during development                                               |
| `package`    | Packages an function for deployment                                                                     |
| `deploy`     | Deploys a function to AWS; will create or update a SAM configuration file if run with option `--guided` |
| `delete` | Deletes your function and all AWS resources created by deploy                                           |

@see `sam --help` for a complete list of commands