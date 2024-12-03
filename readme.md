
# Lambda ECR CI/CD Deployment

## Prerequisites
- AWS Account with permissions for ECR and Lambda.
- GitHub Secrets for AWS credentials (AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_REGION).
- Docker installed (optional for local testing).

## Setup
1. Set Up AWS Credentials
Add `AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_REGION` to your GitHub repository secrets.

## Create ECR Repository
Create an ECR repository using AWS CLI or Console:

``` bash
aws ecr create-repository --repository-name ecr-lambda-repo --region YOUR_REGION
```

## Workflow Overview
The GitHub Actions pipeline (in main.yml) runs on each push:

- Checkout the code.
- Configure AWS credentials.
- Login to ECR.
- Build and push the Docker image to ECR.

## Dockerfile
- Uses `public.ecr.aws/lambda/python:3.10` as base.
- Installs dependencies from requirements.txt.
- Copies lambda_function.py and sets lambda_function.handler as the entry point.

## Lambda Function
``` python
import sys

def handler(event, context):
    return "Hello from lambda using ecr cicd"
```

## Triggering the Workflow
``` bash
git add .
git commit -m "Trigger CI/CD"
git push
````

## Testing the Lambda Function
- In AWS Lambda, create a new function with a container image.
- Use the ECR image URI to deploy the function.

## Cleanup
- Delete Lambda function.
- Delete the ECR repository:
```bash
aws ecr delete-repository --repository-name ecr-lambda-repo --force --region YOUR_REGION
```