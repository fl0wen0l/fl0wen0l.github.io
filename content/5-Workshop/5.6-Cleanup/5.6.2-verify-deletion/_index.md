---
title: "Verify Resource Deletion"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 5.6.2. </b> "
---

Although we configured `RemovalPolicy.DESTROY` and `autoDeleteObjects: true` in our CDK code, it is a **Best Practice** to double-check the AWS Console to ensure everything is gone.

#### A. Check Amazon S3

1. Go to the [S3 Console](https://console.aws.amazon.com/s3/)
2. Search for `findnest` in the bucket list
3. Verify the bucket `findnest-images-...` is deleted

If the bucket still exists:

1. Select the bucket
2. Click **Empty** button
3. Confirm by typing `permanently delete`
4. After emptying, click **Delete** button
5. Confirm by typing the bucket name

{{% notice info %}}
The `autoDeleteObjects: true` setting in our code usually handles this automatically by deploying a custom Lambda function to clear the bucket before deletion.
{{% /notice %}}

#### B. Check DynamoDB Tables

1. Go to the [DynamoDB Console](https://console.aws.amazon.com/dynamodb/)
2. Click **Tables** in the left sidebar
3. Verify that all tables are deleted:
   - BoardingHouseListings
   - UserProfiles
   - OTPVerifications
   - UserFavorites
   - SupportRequests
   - SearchHistory
   - UserPreferences

If any tables remain, select them and click **Delete**.

#### C. Check CloudWatch Logs

1. Go to the [CloudWatch Console](https://console.aws.amazon.com/cloudwatch/)
2. Navigate to **Logs** → **Log groups**
3. Search for `/aws/lambda/FindNestApi`

If the log group persists (sometimes logs generated during deletion are kept):

1. Select the log group
2. Choose **Actions** → **Delete log group(s)**
3. Confirm deletion

{{% notice tip %}}
Log groups might accumulate small charges over time. It's good practice to delete them if no longer needed.
{{% /notice %}}

#### D. Check Lambda Functions

1. Go to the [Lambda Console](https://console.aws.amazon.com/lambda/)
2. Verify that `FindNestApi` function is deleted

If it still exists, select it and click **Actions** → **Delete**.

#### E. Check API Gateway

1. Go to the [API Gateway Console](https://console.aws.amazon.com/apigateway/)
2. Verify that `FindNestAPI` is deleted

If it still exists, select it and click **Actions** → **Delete API**.

#### F. Check Cognito

1. Go to the [Cognito Console](https://console.aws.amazon.com/cognito/)
2. Check **User Pools** - Verify `FindNestUsers` is deleted
3. Check **Identity Pools** - Verify `FindNestMapAccess` is deleted

#### G. Check Location Service

1. Go to the [Amazon Location Service Console](https://console.aws.amazon.com/location/)
2. Verify the following are deleted:
   - Map: `FindNestMap`
   - Place Index: `FindNestPlaces`
   - Route Calculator: `FindNestRoutes`

#### H. Check CloudFormation

1. Go to the [CloudFormation Console](https://console.aws.amazon.com/cloudformation/)
2. Verify that `BackendStack` shows status **DELETE_COMPLETE** or is no longer listed

{{% notice success %}}
If you don't see the stack or it shows DELETE_COMPLETE, your cleanup is successful!
{{% /notice %}}

#### Verification Checklist

Confirm all resources are deleted:

- ✅ S3 Bucket (`findnest-images-*`)
- ✅ DynamoDB Tables (all 7 tables)
- ✅ Lambda Function (`FindNestApi`)
- ✅ API Gateway (`FindNestAPI`)
- ✅ Cognito User Pool (`FindNestUsers`)
- ✅ Cognito Identity Pool (`FindNestMapAccess`)
- ✅ Location Service resources (Map, Place Index, Route Calculator)
- ✅ CloudWatch Log Groups (`/aws/lambda/FindNestApi`)
- ✅ CloudFormation Stack (`BackendStack`)
- ✅ IAM Roles (auto-deleted with stack)

{{% notice warning %}}
If any resources remain after 10 minutes, manually delete them to avoid unexpected charges.
{{% /notice %}}
