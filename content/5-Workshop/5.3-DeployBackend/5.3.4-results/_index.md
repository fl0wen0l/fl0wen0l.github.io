---
title: "Results & Outputs"
date: "`r Sys.Date()`"
weight: 4
chapter: false
pre: " <b> 5.3.4. </b> "
---

Upon successful deployment, the terminal will display the **Outputs** section in green. These are critical parameters for connecting the Frontend to the Backend.

#### Stack Outputs

⚠️ **PLEASE COPY AND SAVE THESE VALUES:**

```plaintext
✅  BackendStack

Outputs:
BackendStack.ApiUrl = https://xyz123.execute-api.us-east-1.amazonaws.com/prod/
BackendStack.UserPoolId = us-east-1_AbCdEfGh
BackendStack.UserPoolClientId = 1a2b3c4d5e6f7g8h9i0j
BackendStack.IdentityPoolId = us-east-1:12345678-abcd-1234-abcd-1234567890ab
BackendStack.ImagesBucket = findnest-images-123456789012
BackendStack.MapName = FindNestMap-123456789012
BackendStack.PlaceIndexName = FindNestPlacesV3-123456789012
BackendStack.RouteCalculatorName = FindNestRoutesV3-123456789012
BackendStack.Region = us-east-1

✅  MonitoringStack

Outputs:
MonitoringStack.AlertTopicArn = arn:aws:sns:us-east-1:123456789012:BoardingHouseAlerts
MonitoringStack.DashboardUrl = https://us-east-1.console.aws.amazon.com/cloudwatch/home?region=us-east-1#dashboards:name=SmartBoardingHouse-Monitoring
```

![Deployment Outputs](/images/5-Workshop/5.3-DeployBackend/outputs.png)

#### Output Parameters Explained

| Parameter               | Description                       | Usage                                                |
| ----------------------- | --------------------------------- | ---------------------------------------------------- |
| **ApiUrl**              | The endpoint for the Backend API  | Used by Frontend to make API calls                   |
| **UserPoolId**          | Cognito User Pool identifier      | Used for user authentication (Login/Register)        |
| **UserPoolClientId**    | Cognito App Client identifier     | Used for user authentication (Login/Register)        |
| **IdentityPoolId**      | Cognito Identity Pool identifier  | Used for displaying maps on the Frontend             |
| **ImagesBucket**        | S3 bucket name for images         | Used for storing and retrieving room images          |
| **MapName**             | Location Service map name         | Used for displaying maps on the Frontend application |
| **PlaceIndexName**      | Location Service place index      | Used for geocoding and search functionality          |
| **RouteCalculatorName** | Location Service route calculator | Used for calculating routes between locations        |
| **Region**              | AWS region                        | The region where resources are deployed              |
| **AlertTopicArn**       | SNS Topic ARN for alerts          | Subscribe to this topic to receive system alerts     |
| **DashboardUrl**        | CloudWatch Dashboard URL          | Access monitoring dashboard to view system metrics   |

#### Next Steps

Save these outputs in a secure location. You will need them to configure the Frontend application in the next section.

{{% notice tip %}}
You can also retrieve these outputs later by running `cdk deploy` again or checking the CloudFormation stack outputs in the AWS Console.
{{% /notice %}}

#### Congratulations

You have successfully deployed a modern **Serverless Backend system**, integrated with:

- ✅ AI (Amazon Bedrock with Claude 3)
- ✅ Digital Maps (Amazon Location Service)
- ✅ Strict security permissions (IAM)
- ✅ Scalable database (DynamoDB)
- ✅ User authentication (Cognito)
- ✅ RESTful API (API Gateway + Lambda)
- ✅ Comprehensive monitoring (CloudWatch Dashboard & Alarms)
- ✅ Real-time alerts (SNS Notifications)
