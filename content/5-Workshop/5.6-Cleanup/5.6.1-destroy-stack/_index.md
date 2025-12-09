---
title: "Destroy the Stack"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 5.6.1. </b> "
---

The beauty of **Infrastructure as Code (IaC)** with AWS CDK is that you can dismantle the entire system with a single command.

CDK knows exactly what it created and will delete resources in the correct dependency order.

#### Navigate to CDK Directory

Make sure you are in the `cdk` directory:

```bash
cd cdk
```

#### Run Destroy Command

Execute the following command:

```bash
cdk destroy
```

#### Confirmation Prompt

The CLI will ask for confirmation:

```plaintext
Are you sure you want to delete: BackendStack (y/n)?
```

Type `y` and press **Enter**.

#### Deletion Process

The deletion process generally takes **3-5 minutes**.

You will see output similar to:

```plaintext
BackendStack: destroying...

 ✅  BackendStack: destroyed

Stack ARN:
arn:aws:cloudformation:us-east-1:123456789012:stack/BackendStack/...
```

#### What Gets Deleted

CDK will automatically delete all resources in the correct order:

1. **API Gateway** - REST API and deployment
2. **Lambda Function** - Including associated IAM roles
3. **DynamoDB Tables** - All 7 tables
4. **Cognito User Pool** - Users and groups
5. **Cognito Identity Pool** - Identity mappings
6. **S3 Bucket** - Including all objects (due to `autoDeleteObjects: true`)
7. **Location Service** - Maps, Place Index, Route Calculator
8. **CloudWatch Log Groups** - Lambda execution logs
9. **IAM Roles and Policies** - All service permissions
10. **CloudFormation Stack** - The stack itself

{{% notice info %}}
The `RemovalPolicy.DESTROY` configuration in our CDK code ensures that data-containing resources (like DynamoDB tables and S3 buckets) are also deleted.
{{% /notice %}}

#### Monitor Deletion Progress

You can monitor the deletion progress in:

- **Terminal**: Watch the CLI output
- **AWS Console**: Go to CloudFormation → Stacks → BackendStack → Events tab

#### Troubleshooting Deletion Issues

If the deletion fails:

**Issue: S3 Bucket Not Empty**

```bash
# Manually empty the bucket
aws s3 rm s3://findnest-images-YOUR-ACCOUNT-ID --recursive

# Then retry destroy
cdk destroy
```

**Issue: DynamoDB Table Has Deletion Protection**

If you modified the tables manually in console and enabled deletion protection, you need to disable it first:

1. Go to DynamoDB Console
2. Select the table
3. Go to **Additional settings** tab
4. Turn off **Deletion protection**
5. Retry `cdk destroy`

**Issue: Lambda Execution Role In Use**

Wait a few minutes for Lambda executions to complete, then retry.

{{% notice warning %}}
Once you run `cdk destroy` and confirm, the process cannot be undone. All data will be permanently deleted.
{{% /notice %}}
