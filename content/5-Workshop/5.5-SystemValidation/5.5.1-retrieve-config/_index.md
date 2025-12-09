---
title: "Retrieve Configuration"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 5.5.1. </b> "
---

Ensure you have the **ApiUrl** from the outputs of Section 5.3 (Deploy Backend).

#### Locate the API URL

After running `cdk deploy`, you should have received outputs similar to:

```plaintext
✅  BackendStack

Outputs:
BackendStack.ApiUrl = https://xyz123.execute-api.us-east-1.amazonaws.com/prod/
BackendStack.UserPoolId = us-east-1_AbCdEfGh
...
```

#### API URL Format

The API URL follows this format:

```
https://<api-id>.execute-api.<region>.amazonaws.com/prod/
```

**Example:**

```
https://xyz123.execute-api.us-east-1.amazonaws.com/prod/
```

{{% notice info %}}
The `/prod/` at the end indicates the deployment stage. This is where your API Gateway routes all incoming requests.
{{% /notice %}}

#### Save the API URL

Copy and save this URL. You will use it as the base URL for all API requests in the following test cases.

{{% notice tip %}}
If you lost the outputs, you can retrieve them by:

- Running `cdk deploy` again (it won't redeploy if nothing changed)
- Checking the CloudFormation stack outputs in AWS Console
- Going to API Gateway Console and finding your API's invoke URL
  {{% /notice %}}

#### Verify API Gateway

You can quickly verify that API Gateway is accessible by opening the URL in a browser:

```
https://xyz123.execute-api.us-east-1.amazonaws.com/prod/
```

You should see a response indicating the API is running (might be a 404 or a welcome message, depending on your root route configuration).
