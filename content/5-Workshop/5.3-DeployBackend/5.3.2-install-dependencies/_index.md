---
title: "Install Dependencies"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

The system consists of two parts of source code requiring dependencies.

#### Step 1: Install for Backend

Navigate to the backend Lambda directory and install the required Node.js packages:

```bash
cd backend/src/lambda
npm install
```

This will install all dependencies defined in the `package.json` file for the Lambda function.

#### Step 2: Install for CDK

Go back to the CDK root directory and install CDK dependencies:

```bash
# Go back to CDK root directory
cd ../../../cdk
npm install
```

This installs the AWS CDK libraries and constructs needed to synthesize and deploy the infrastructure.

{{% notice tip %}}
Make sure you have Node.js and npm installed on your machine before running these commands.
{{% /notice %}}
