# Things to consider before starting with SAM

There are several things to consider before you start to deploy your lambdas with SAM.

## Create an AWS S3 bucket to store your SAM templates

SAM will create a managed S3 bucket to store all SAM templates. Since the naming of
the S3 bucket is not very comprehensible, creating a well-named S3 bucket upfront is a good 
idea.

!!! info 

## Create an AWS ECR instance to store your containerized lambdas

SAM will create a managed ECR instance to store all Docker images of your lambdas
(if you chose to use Docker packaging). Since the naming of
the ECR instance is not very comprehensible, creating a well-named ECR instance upfront is a good
idea.

