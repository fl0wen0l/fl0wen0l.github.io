---
title: "Setup the Script"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

#### Step 1: Navigate to the directory

Navigate to the scripts directory where the seeding script is located:

```bash
cd backend/scripts
```

#### Step 2: Install dependencies

The script uses the **AWS SDK v3**. Install the required packages:

```bash
npm install
```

This will install all dependencies defined in the `package.json` file, including:

- `@aws-sdk/client-cognito-identity-provider` - For Cognito operations
- `@aws-sdk/client-dynamodb` - For DynamoDB operations
- `dotenv` - For environment variable management
- Other required dependencies

{{% notice info %}}
Make sure you're in the `backend/scripts` directory before running `npm install`.
{{% /notice %}}

Wait for the installation to complete successfully before proceeding to the next step.
