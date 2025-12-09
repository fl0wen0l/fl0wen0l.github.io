---
title: "Verify Resources"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: " <b> 5.5.5. </b> "
---

#### Verify Resources in Console

If all requests succeed, let's verify the data persistence in AWS Console.

This confirms that data is actually being stored in DynamoDB, not just returned from Lambda's memory.

#### Step 1: Navigate to DynamoDB Console

1. Go to [DynamoDB Console](https://console.aws.amazon.com/dynamodb/)
2. Select **Tables** from the left sidebar

You should see all the tables created by your CDK stack:

- BoardingHouseListings
- UserProfiles
- OTPVerifications
- UserFavorites
- SupportRequests
- SearchHistory
- UserPreferences

#### Step 2: Explore the Listings Table

1. Click on the **BoardingHouseListings** table
2. Click **Explore table items** button

You should see the listing you created in Test Case 2.

#### Step 3: Verify the Data

Check that the record contains:

- **listingId**: Auto-generated unique identifier
- **title**: "Luxury Apartment in District 1"
- **description**: "Full furniture, near manufacturing hub"
- **price**: 5000000
- **address**: "123 Le Loi, District 1, HCMC"
- **area**: 45
- **amenities**: Array containing ["Wifi", "AC", "Parking"]
- **createdAt**: Timestamp of creation
- **landlordId**: User ID of the admin who created it

#### Step 4: Verify User Profile

1. Go back to Tables
2. Click on **UserProfiles** table
3. Click **Explore table items**

You should see the admin user profile created in Section 5.4:

- **userId**: Cognito user ID
- **email**: Your admin email
- **fullName**: Admin's full name
- **userType**: "admin"
- **createdAt**: Timestamp

#### Verification Checklist

Confirm the following:

- ✅ **API Gateway** is routing requests correctly
- ✅ **Lambda** is processing requests and executing business logic
- ✅ **Cognito** is authenticating users successfully
- ✅ **DynamoDB** is storing and retrieving data correctly
- ✅ **Protected routes** require authentication
- ✅ **Public routes** are accessible without authentication
- ✅ **IAM permissions** allow Lambda to access all necessary services

#### System Architecture Validation

You have successfully validated a complete serverless architecture:

```
Client (Postman/cURL)
    ↓
API Gateway (REST API)
    ↓
Lambda Function (Node.js)
    ↓
├─→ Cognito (Authentication)
├─→ DynamoDB (Data Storage)
├─→ SNS (Notifications - not tested)
├─→ Bedrock (AI - not tested)
└─→ Location Service (Maps - not tested)
```

{{% notice success %}}
Congratulations! Your serverless backend is fully functional and ready for frontend integration.
{{% /notice %}}

#### Next Steps

- Test additional endpoints (user registration, favorites, search, etc.)
- Monitor Lambda execution in CloudWatch Logs
- Set up CloudWatch alarms for errors
- Integrate with a frontend application
- Implement additional features (AI chat, map integration, notifications)

{{% notice tip %}}
Keep your API URL and admin credentials secure. Consider using environment variables or secrets management in production.
{{% /notice %}}
