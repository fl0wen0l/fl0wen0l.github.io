---
title: "Architecture Deep Dive"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

Let's dive into the **BackendStack** source code to understand how the system operates. Below are the detailed code snippets for each resource.

#### 1. Database (Amazon DynamoDB)

We initialize 6 DynamoDB tables using `PAY_PER_REQUEST` (On-Demand) mode to optimize costs and scalability.

```typescript
// 1. Listings Table (Room rentals)
const listingsTable = new dynamodb.Table(this, "ListingsTable", {
  tableName: "BoardingHouseListings",
  partitionKey: { name: "listingId", type: dynamodb.AttributeType.STRING },
  billingMode: dynamodb.BillingMode.PAY_PER_REQUEST,
  removalPolicy: RemovalPolicy.DESTROY,
});

// 2. User Profiles Table
const userProfilesTable = new dynamodb.Table(this, "UserProfilesTable", {
  tableName: "UserProfiles",
  partitionKey: { name: "userId", type: dynamodb.AttributeType.STRING },
  billingMode: dynamodb.BillingMode.PAY_PER_REQUEST,
  removalPolicy: RemovalPolicy.DESTROY,
});

// 3. OTP Table (Phone Verification) - Auto-delete after 5 mins (TTL)
const otpTable = new dynamodb.Table(this, "OTPVerifications", {
  tableName: "OTPVerifications",
  partitionKey: {
    name: "phoneNumber",
    type: dynamodb.AttributeType.STRING,
  },
  billingMode: dynamodb.BillingMode.PAY_PER_REQUEST,
  timeToLiveAttribute: "ttl", // TTL configuration
  removalPolicy: RemovalPolicy.DESTROY,
});

// 4. Favorites Table - Includes Sort Key for quick lookup by user
const favoritesTable = new dynamodb.Table(this, "FavoritesTable", {
  tableName: "UserFavorites",
  partitionKey: { name: "userId", type: dynamodb.AttributeType.STRING },
  sortKey: { name: "listingId", type: dynamodb.AttributeType.STRING },
  billingMode: dynamodb.BillingMode.PAY_PER_REQUEST,
  removalPolicy: RemovalPolicy.DESTROY,
});

// 5. Support Requests Table
const supportRequestsTable = new dynamodb.Table(this, "SupportRequestsTable", {
  tableName: "SupportRequests",
  partitionKey: {
    name: "requestId",
    type: dynamodb.AttributeType.STRING,
  },
  billingMode: dynamodb.BillingMode.PAY_PER_REQUEST,
  removalPolicy: RemovalPolicy.DESTROY,
});

// 6. Notifications Table - Includes TTL
const notificationsTable = new dynamodb.Table(this, "NotificationsTable", {
  tableName: "Notifications",
  partitionKey: { name: "userId", type: dynamodb.AttributeType.STRING },
  sortKey: { name: "notificationId", type: dynamodb.AttributeType.STRING },
  billingMode: dynamodb.BillingMode.PAY_PER_REQUEST,
  timeToLiveAttribute: "ttl",
  removalPolicy: RemovalPolicy.DESTROY,
});
```

#### 2. Storage (Amazon S3)

An S3 Bucket is created to store room images, configured with security blocks against public access and auto-deletion when the stack is destroyed.

```typescript
const imagesBucket = new s3.Bucket(this, "BoardingHouseImages", {
  bucketName: `findnest-images-${cdk.Aws.ACCOUNT_ID}`, // Unique bucket name
  removalPolicy: RemovalPolicy.DESTROY,
  autoDeleteObjects: true,
  blockPublicAccess: s3.BlockPublicAccess.BLOCK_ALL, // Private by default
});
```

#### 3. Authentication (Amazon Cognito)

We configure a User Pool to manage users and an Identity Pool to grant the Frontend direct access to AWS resources.

```typescript
// Create User Pool
const userPool = new cognito.UserPool(this, "UserPool", {
  userPoolName: "FindNestUsers",
  selfSignUpEnabled: false, // User created via API (Backend trigger)
  signInAliases: {
    phone: true, // User uses Phone Number
    username: true, // Admin uses Username
  },
  autoVerify: { phone: true },
  standardAttributes: {
    email: { required: false, mutable: true },
    phoneNumber: { required: false, mutable: true },
  },
  passwordPolicy: {
    minLength: 8,
    requireLowercase: true,
    requireUppercase: true,
    requireDigits: true,
    requireSymbols: true,
  },
  accountRecovery: cognito.AccountRecovery.PHONE_ONLY_WITHOUT_MFA,
  removalPolicy: RemovalPolicy.DESTROY,
});

// Create Client App
const userPoolClient = userPool.addClient("UserPoolClient", {
  authFlows: {
    userPassword: true,
    adminUserPassword: true, // Used for Backend auth flow
    custom: true,
  },
});

// User Groups
const usersGroup = new cognito.CfnUserPoolGroup(this, "UsersGroup", {
  userPoolId: userPool.userPoolId,
  groupName: "Users",
});

const landlordsGroup = new cognito.CfnUserPoolGroup(this, "LandlordsGroup", {
  userPoolId: userPool.userPoolId,
  groupName: "Landlords",
});

const adminsGroup = new cognito.CfnUserPoolGroup(this, "AdminsGroup", {
  userPoolId: userPool.userPoolId,
  groupName: "Admins",
});

// Identity Pool for Frontend
const identityPool = new cognito.CfnIdentityPool(this, "IdentityPool", {
  identityPoolName: "FindNestMapAccess",
  allowUnauthenticatedIdentities: true, // Allow guests to view map
  cognitoIdentityProviders: [
    {
      clientId: userPoolClient.userPoolClientId,
      providerName: userPool.userPoolProviderName,
    },
  ],
});

// IAM Role for unauthenticated users (map access only)
const unauthenticatedRole = new iam.Role(this, "UnauthenticatedRole", {
  assumedBy: new iam.FederatedPrincipal(
    "cognito-identity.amazonaws.com",
    {
      StringEquals: {
        "cognito-identity.amazonaws.com:aud": identityPool.ref,
      },
      "ForAnyValue:StringLike": {
        "cognito-identity.amazonaws.com:amr": "unauthenticated",
      },
    },
    "sts:AssumeRoleWithWebIdentity"
  ),
  inlinePolicies: {
    LocationServicePolicy: new iam.PolicyDocument({
      statements: [
        new iam.PolicyStatement({
          effect: iam.Effect.ALLOW,
          actions: [
            "geo:GetMap*",
            "geo:SearchPlaceIndexForText",
            "geo:SearchPlaceIndexForPosition",
            "geo:GetPlace",
            "geo:CalculateRoute",
            "geo:CalculateRouteMatrix",
          ],
          resources: [map.attrArn, placeIndex.attrArn, routeCalculator.attrArn],
        }),
      ],
    }),
  },
});

// IAM Role for authenticated users (full access)
const authenticatedRole = new iam.Role(this, "AuthenticatedRole", {
  assumedBy: new iam.FederatedPrincipal(
    "cognito-identity.amazonaws.com",
    {
      StringEquals: {
        "cognito-identity.amazonaws.com:aud": identityPool.ref,
      },
      "ForAnyValue:StringLike": {
        "cognito-identity.amazonaws.com:amr": "authenticated",
      },
    },
    "sts:AssumeRoleWithWebIdentity"
  ),
  inlinePolicies: {
    LocationServicePolicy: new iam.PolicyDocument({
      statements: [
        new iam.PolicyStatement({
          effect: iam.Effect.ALLOW,
          actions: [
            "geo:GetMap*",
            "geo:SearchPlaceIndexForText",
            "geo:SearchPlaceIndexForPosition",
            "geo:GetPlace",
            "geo:CalculateRoute",
            "geo:CalculateRouteMatrix",
            "geo:BatchGetDevicePosition",
            "geo:GetDevicePosition",
          ],
          resources: [map.attrArn, placeIndex.attrArn, routeCalculator.attrArn],
        }),
      ],
    }),
  },
});

// Attach roles to identity pool
new cognito.CfnIdentityPoolRoleAttachment(this, "IdentityPoolRoleAttachment", {
  identityPoolId: identityPool.ref,
  roles: {
    authenticated: authenticatedRole.roleArn,
    unauthenticated: unauthenticatedRole.roleArn,
  },
});
```

#### 4. Location Service (Maps & Geocoding)

Initialize Location Service resources using **Here** data provider for better POI coverage in Vietnam.

```typescript
const placeIndex = new location.CfnPlaceIndex(this, "PlaceIndex", {
  indexName: `FindNestPlacesV3-${cdk.Aws.ACCOUNT_ID}`,
  dataSource: "Here", // Better POI coverage for Asia (Vietnam)
  dataSourceConfiguration: {
    intendedUse: "Storage", // Allows storing and querying POI data
  },
});

const map = new location.CfnMap(this, "Map", {
  mapName: `FindNestMap-${cdk.Aws.ACCOUNT_ID}`,
  configuration: { style: "VectorEsriStreets" },
});

const routeCalculator = new location.CfnRouteCalculator(
  this,
  "RouteCalculator",
  {
    calculatorName: `FindNestRoutesV3-${cdk.Aws.ACCOUNT_ID}`,
    dataSource: "Here",
  }
);
```

#### 5. Compute (AWS Lambda Monolith)

Configure the Lambda Function containing the entire Backend logic, including full environment variable injection.

```typescript
const apiLambda = new lambda.Function(this, "ApiLambda", {
  functionName: "FindNestApi",
  runtime: lambda.Runtime.NODEJS_20_X,
  handler: "index.handler",
  code: lambda.Code.fromAsset(path.join(__dirname, "../../backend/src/lambda")),
  timeout: cdk.Duration.seconds(30),
  logGroup: logGroup,
  environment: {
    // Environment variables connecting resources
    LISTINGS_TABLE_NAME: listingsTable.tableName,
    USER_PROFILES_TABLE_NAME: userProfilesTable.tableName,
    OTP_TABLE_NAME: otpTable.tableName,
    FAVORITES_TABLE_NAME: favoritesTable.tableName,
    SUPPORT_REQUESTS_TABLE_NAME: supportRequestsTable.tableName,
    NOTIFICATIONS_TABLE_NAME: notificationsTable.tableName,
    IMAGES_BUCKET_NAME: imagesBucket.bucketName,
    USER_POOL_ID: userPool.userPoolId,
    USER_POOL_CLIENT_ID: userPoolClient.userPoolClientId,
    PLACE_INDEX_NAME: placeIndex.indexName,
    MAP_NAME: map.mapName,
    ROUTE_CALCULATOR_NAME: routeCalculator.calculatorName,
    BEDROCK_MODEL_ID: "anthropic.claude-3-sonnet-20240229-v1:0",
    REGION: cdk.Aws.REGION,
  },
});
```

#### 6. Permissions (Granular Access Control)

We grant **Granular Permissions** to the Lambda Function. Here is the full list of granted permissions:

**A. Read/Write Access to Database and S3:**

```typescript
listingsTable.grantReadWriteData(apiLambda);
userProfilesTable.grantReadWriteData(apiLambda);
otpTable.grantReadWriteData(apiLambda);
favoritesTable.grantReadWriteData(apiLambda);
supportRequestsTable.grantReadWriteData(apiLambda);
notificationsTable.grantReadWriteData(apiLambda);
imagesBucket.grantReadWrite(apiLambda);
```

**B. User Management in Cognito (Full Admin Actions):**

```typescript
apiLambda.addToRolePolicy(
  new iam.PolicyStatement({
    actions: [
      "cognito-idp:AdminCreateUser",
      "cognito-idp:AdminSetUserPassword",
      "cognito-idp:AdminInitiateAuth",
      "cognito-idp:AdminGetUser",
      "cognito-idp:AdminAddUserToGroup",
      "cognito-idp:AdminRemoveUserFromGroup",
      "cognito-idp:AdminListGroupsForUser",
      "cognito-idp:AdminUpdateUserAttributes",
      "cognito-idp:AdminEnableUser",
      "cognito-idp:AdminDisableUser",
      "cognito-idp:AdminDeleteUser",
      "cognito-idp:ListUsers",
      "cognito-idp:GlobalSignOut",
    ],
    resources: [userPool.userPoolArn],
  })
);
```

**C. Integrations with other services (SNS, Bedrock, Location):**

```typescript
// Send SMS (OTP)
apiLambda.addToRolePolicy(
  new iam.PolicyStatement({
    actions: ["sns:Publish"],
    resources: ["*"],
  })
);

// Invoke AI (Claude 3)
apiLambda.addToRolePolicy(
  new iam.PolicyStatement({
    actions: ["bedrock:InvokeModel"],
    resources: ["arn:aws:bedrock:*::foundation-model/anthropic.claude-3-*"],
  })
);

// Access Location Service
apiLambda.addToRolePolicy(
  new iam.PolicyStatement({
    actions: [
      "geo:SearchPlaceIndexForText",
      "geo:GetPlace",
      "geo:CalculateRoute",
      "geo:SearchPlaceIndexForPosition",
    ],
    resources: [placeIndex.attrArn, routeCalculator.attrArn],
  })
);
```

#### 7. API Gateway (REST API)

Create a public Endpoint for clients to call the Lambda function.

```typescript
const api = new apigateway.LambdaRestApi(this, "BoardingHouseApi", {
  handler: apiLambda,
  proxy: true,
  deployOptions: {
    stageName: "prod",
    throttlingBurstLimit: 100,
    throttlingRateLimit: 50,
  },
  defaultCorsPreflightOptions: {
    allowOrigins: apigateway.Cors.ALL_ORIGINS,
    allowMethods: apigateway.Cors.ALL_METHODS,
    allowHeaders: ["Content-Type", "Authorization"],
  },
  restApiName: "FindNestAPI",
});
```

#### 8. Monitoring & Observability (CloudWatch)

We implement comprehensive monitoring with **CloudWatch Dashboard**, **Alarms**, and **SNS Notifications**.

**A. SNS Alert Topic:**

```typescript
const alertTopic = new sns.Topic(this, "AlertTopic", {
  topicName: "BoardingHouseAlerts",
  displayName: "Smart Boarding House Alerts",
});

alertTopic.addSubscription(
  new snsSubscriptions.EmailSubscription("admin@smartboardinghouse.com")
);
```

**B. CloudWatch Alarms:**

```typescript
// Lambda Error Alarm
const lambdaErrorAlarm = new cloudwatch.Alarm(this, "LambdaErrorAlarm", {
  alarmName: "BoardingHouse-Lambda-Errors",
  metric: new cloudwatch.Metric({
    namespace: "AWS/Lambda",
    metricName: "Errors",
    dimensionsMap: { FunctionName: lambdaFunctionName },
    statistic: "Sum",
  }),
  threshold: 5,
  evaluationPeriods: 2,
});

// Lambda Duration Alarm
const lambdaDurationAlarm = new cloudwatch.Alarm(this, "LambdaDurationAlarm", {
  alarmName: "BoardingHouse-Lambda-Duration",
  metric: new cloudwatch.Metric({
    namespace: "AWS/Lambda",
    metricName: "Duration",
    dimensionsMap: { FunctionName: lambdaFunctionName },
    statistic: "Average",
  }),
  threshold: 25000, // 25 seconds
  evaluationPeriods: 3,
});

// API Gateway 4xx/5xx Alarms
const apiGateway4xxAlarm = new cloudwatch.Alarm(this, "ApiGateway4xxAlarm", {
  alarmName: "BoardingHouse-API-4xx-Errors",
  metric: new cloudwatch.Metric({
    namespace: "AWS/ApiGateway",
    metricName: "4XXError",
    dimensionsMap: { ApiName: apiGatewayName },
    statistic: "Sum",
  }),
  threshold: 10,
  evaluationPeriods: 2,
});
```

**C. CloudWatch Dashboard (8 Rows):**

```typescript
const dashboard = new cloudwatch.Dashboard(this, "BoardingHouseDashboard", {
  dashboardName: "SmartBoardingHouse-Monitoring",
  defaultInterval: cdk.Duration.hours(24),
});

// Row 1: Lambda Overview (Invocations, Errors, Throttles)
dashboard.addWidgets(
  new cloudwatch.GraphWidget({
    title: "Lambda Invocations",
    left: [lambdaInvocationsMetric],
    width: 8,
    height: 6,
  }),
  new cloudwatch.GraphWidget({
    title: "Lambda Errors",
    left: [lambdaErrorsMetric],
    width: 8,
    height: 6,
  }),
  new cloudwatch.GraphWidget({
    title: "Lambda Throttles",
    left: [lambdaThrottlesMetric],
    width: 8,
    height: 6,
  })
);

// Row 2: Lambda Performance (Duration, Concurrent Executions)
// Row 3: API Gateway (Requests, Latency)
// Row 4: API Gateway Errors (4XX, 5XX)
// Row 5: DynamoDB Metrics (Read/Write Capacity, Errors)
// Row 6: System Health Summary (Error Rates, Response Time, Total Requests)
// Row 7: Bedrock AI Token Usage & Invocations
// Row 8: Bedrock Model Summary (Total Tokens, Invocations, Latency)
```

**Dashboard Features:**

- 📊 **Lambda Metrics**: Invocations, Errors, Throttles, Duration, Concurrency
- 🌐 **API Gateway Metrics**: Requests, Latency (Avg & P99), 4XX/5XX Errors
- 💾 **DynamoDB Metrics**: Read/Write Capacity, User Errors per table
- 🤖 **Bedrock AI Metrics**: Token Usage (Input/Output), Invocations, Latency
- 📈 **System Health Summary**: Error Rates, Response Time, Total Requests (24h)
- 🚨 **Automatic Alerts**: Email notifications via SNS when thresholds are breached
