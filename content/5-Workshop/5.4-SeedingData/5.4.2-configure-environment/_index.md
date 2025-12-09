---
title: "Configure Environment"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

The script (`seed-admin.js`) looks for specific environment variables to connect to your AWS resources. You need to create a `.env` file with values from the CDK Deployment Output.

#### Create the .env file

Create a file named `.env` in the `backend/scripts` folder with the following content:

```bash
# .env file content
REGION=us-east-1
USER_POOL_ID=<Paste_Value_From_CDK_Output>
USER_PROFILES_TABLE_NAME=UserProfiles
```

#### Environment Variables Explained

| Variable                     | Description                             | Where to find                                                                                                               |
| ---------------------------- | --------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| **REGION**                   | AWS region where resources are deployed | `us-east-1` (or `ap-southeast-1` if you changed it)                                                                         |
| **USER_POOL_ID**             | Cognito User Pool identifier            | Copy the value of `BackendStack.UserPoolId` from the terminal after `cdk deploy`                                            |
| **USER_PROFILES_TABLE_NAME** | DynamoDB table name for user profiles   | In your CDK code, we set the table name to `UserProfiles`. If you changed it, check the DynamoDB Console for the exact name |

#### Example Configuration

```bash
# Example .env file
REGION=us-east-1
USER_POOL_ID=us-east-1_AbCdEfGh
USER_PROFILES_TABLE_NAME=UserProfiles
```

{{% notice warning %}}
Make sure to replace `<Paste_Value_From_CDK_Output>` with the actual value from your CDK deployment outputs!
{{% /notice %}}

{{% notice tip %}}
You can find the CDK outputs by scrolling up in your terminal or by running `cdk deploy` again (it won't redeploy if nothing changed).
{{% /notice %}}
