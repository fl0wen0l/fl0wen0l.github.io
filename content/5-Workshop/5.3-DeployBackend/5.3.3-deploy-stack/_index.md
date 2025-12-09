---
title: "Deploy Stack"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

Now, let's turn this design into reality.

#### Step 1: Synthesize

Run the synthesize command to generate the CloudFormation template:

```bash
cdk synth
```

This command generates the CloudFormation template in the `cdk.out` directory. If no red errors appear, you may proceed.

{{% notice info %}}
The synthesize process converts your CDK code (TypeScript) into a CloudFormation template (JSON/YAML).
{{% /notice %}}

#### Step 2: Deploy

Deploy the stack to AWS:

```bash
cdk deploy --require-approval never
```

The `--require-approval never` flag bypasses the confirmation prompt for IAM changes.

{{% notice warning %}}
The deployment process will take approximately **5-10 minutes** to provision all resources.
{{% /notice %}}

During deployment, you'll see:

- CloudFormation stack creation progress
- Individual resources being created (DynamoDB tables, Lambda functions, API Gateway, etc.)
- Status updates for each resource

Wait for the deployment to complete successfully before proceeding to the next step.
