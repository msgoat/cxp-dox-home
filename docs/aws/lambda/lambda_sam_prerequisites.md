# Things to consider before starting with SAM

There are several things to consider before you start to deploy your lambdas with SAM.

## Create an AWS S3 bucket to store your SAM templates

SAM will create a managed S3 bucket to store all SAM templates. Since the naming of
the S3 bucket is not very comprehensible, creating a well-named S3 bucket upfront is a good 
idea.

In order to tell SAM to use a previously create S3 bucket, make sure the SAM configuration file contains the following settings:

````toml
[default.deploy.parameters]
s3_bucket = "${s3BucketName}"
s3_prefix = "${lambdaFunctionProjectName}"
resolve_s3 = false
````
with:
* `${s3BucketName}` : Name of an existing S3 bucket
* `${lambdaFunctionProjectName}` : S3 prefix to use for your lambda function artifacts (preferably the name of your lambda function project)

## Create an AWS ECR instance to store your containerized lambdas

SAM will create a managed ECR instance to store all Docker images of your lambdas
(if you chose to use Docker packaging). Since the naming of
the ECR instance is not very comprehensible, creating a well-named ECR instance upfront is a good
idea.

In order to tell SAM to use a previously created ECR repository bucket, make sure the SAM configuration file contains the following settings:

````toml
[default.deploy.parameters]
image_repositories = ["${lambdaFunctionName}=${ecrRepoUri}"]
resolve_image_repos = false
````
with:
* `${lambdaFunctionName}` : Name of the lambda function as defined in the SAM template file
* `${ecrRepoUri}` : URI of an existing ECR repository

