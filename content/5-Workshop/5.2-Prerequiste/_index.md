---
title: "Prerequiste"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

#### IAM permissions

To deploy the Findnest Backend stack securely, adhering to the principle of least privilege, add the following JSON policy to your IAM User.

This policy focuses strictly on the services defined in your CDK code (DynamoDB, Cognito, Location, etc.)

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "FindnestDeploymentPermissions",
      "Effect": "Allow",
      "Action": [
        "cloudformation:*",
        "s3:*",
        "iam:*",
        "lambda:*",
        "apigateway:*",
        "dynamodb:*",
        "cognito-idp:*",
        "cognito-identity:*",
        "geo:*",
        "sns:*",
        "bedrock:*",
        "cloudwatch:*",
        "logs:*",
        "ssm:GetParameter",
        "sts:AssumeRole"
      ],
      "Resource": "*"
    }
  ]
}
```

#### Provision resources (CDK Bootstrap)

In this lab, we will use N. Virginia region (`us-east-1`).
To prepare the workshop environment, we need to provision the CDK Bootstrap resources. This process creates an S3 Bucket to store your Lambda code and CloudFormation templates.

**1. Install Dependencies**

Ensure you have the core tools installed on your local machine:

```bash
npm install -g aws-cdk
```

**2. Bootstrap the Environment**

Run the following command in your terminal to deploy the bootstrap stack. Accept all of the defaults.

```bash
cdk bootstrap aws://<YOUR-ACCOUNT-ID>/us-east-1
```

(Replace `<YOUR-ACCOUNT-ID>` with your 12-digit AWS Account ID)

Wait for the CloudFormation stack `CDKToolkit` to reach `CREATE_COMPLETE` status.
1 S3 Bucket has been created to store assets.
The environment is now ready for serverless deployment.
