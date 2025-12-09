---
title: "Blog 3"
date: "2025-11-25"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# Automating email notifications for admin teams working with Amazon SageMaker Catalog

**Authors:** Himanshu Sahni, Jitesh Kumar, and Rajiv Upadhyay | **Date:** 21/10/2025

[Amazon SageMaker Catalog](https://aws.amazon.com/sagemaker/catalog/) simplifies discovery, governance, and collaboration for data and AI across the Data Lakehouse, model catalogs, and applications. With Amazon SageMaker Catalog, you can securely discover and access approved data and models using semantic search powered by AI-generated metadata — or simply ask [Amazon Q Developer](https://aws.amazon.com/q/developer/) in natural language to find your data.

Large enterprise customers often operate multiple business domains that produce and consume data using a centralized SageMaker Data Catalog. Many customers maintain a central data governance team responsible for creating, publishing, and maintaining governance standards and best practices company-wide. As the customer's data platform scales, it becomes challenging for a central governance team to enforce standards across all data producers and consumers. Therefore, many governance teams need observability into user activity within Amazon SageMaker Catalog to ensure published data assets comply with organization governance standards and best practices. In this scenario, automation is required so the central governance team can be notified when important events occur in Amazon SageMaker Catalog.

In this post, we show how to create custom notifications for events that occur in SageMaker Catalog using [Amazon EventBridge](https://aws.amazon.com/eventbridge/), [AWS Lambda](https://aws.amazon.com/lambda/), and [Amazon Simple Notification Service (Amazon SNS)](https://aws.amazon.com/sns/). You can extend this solution to automatically integrate SageMaker Catalog with internal enterprise workflow tools such as ServiceNow and Helix.

## Solution overview

The following solution architecture shows how SageMaker Catalog integrates with other AWS services such as [AWS IAM Identity Center](https://aws.amazon.com/iam/identity-center/), Amazon EventBridge, [Amazon SQS](https://aws.amazon.com/sqs/), AWS Lambda, and Amazon SNS to create automated notifications that record important catalog events for the enterprise.

![Solution architecture](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/12/Screenshot-2025-10-12-at-10.43.50%20AM.png)

1. A SageMaker Catalog user signs in to [Amazon SageMaker Unified Studio](https://aws.amazon.com/sagemaker/unified-studio/) via IAM Identity Center. This user might be a data scientist, ML engineer, or analyst searching for published datasets in the organization. AWS IAM Identity Center ensures only authorized personnel can access classified assets and ML resources.
2. The user performs an action in SageMaker Catalog. For example: creating a new project, or searching for a data asset and creating a registration request to access the asset.
3. User events from SageMaker Catalog are recorded in Amazon EventBridge. Amazon EventBridge is a serverless, fully managed event bus service that helps you build scalable event-driven applications across AWS, SaaS, and custom apps. EventBridge enables filtering of events and allows you to act on specific events. An example EventBridge pattern that filters create-project events from DataZone looks like this:

```json
{
  "source": [
    "aws.datazone"
  ],
  "detail": {
    "eventSource": [
      "datazone.amazonaws.com"
    ],
    "eventName": [
      "CreateProject"
    ]
  }
}
```

4. Amazon EventBridge sends filtered events to an Amazon SQS queue. Routing events to SQS increases reliability and durability. Amazon SQS acts as a buffer between EventBridge and AWS Lambda, decoupling event producers from consumers. This lets your Lambda functions process messages at their own rate, preventing overload during traffic bursts or when downstream resources are temporarily slow or unavailable. SQS provides durable storage for events — if Lambda is unavailable or throttled, messages remain in the queue until they can be processed successfully, reducing the risk of data loss. A Dead Letter Queue (DLQ) is attached to the primary SQS queue. Attaching a DLQ ensures any messages that cannot be processed after retries are safely recorded for analysis and remediation instead of blocking or circulating indefinitely in the primary queue.
5. An AWS Lambda function reads messages from the SQS queue and formats notifications according to your needs.
6. The Lambda function publishes messages to Amazon SNS. End users and central governance teams can subscribe to the SNS topic to receive email alerts when events occur in the SageMaker catalog.
7. Amazon CloudWatch integrates with AWS Lambda to monitor performance, log events, and trigger alarms if issues occur, ensuring your workflow runs smoothly.

## Prerequisites

You must set up the following prerequisite resources:

* An AWS account with [Amazon Virtual Private Cloud (VPC)](https://aws.amazon.com/vpc/) and networking configured.
* An existing SageMaker Unified Studio domain (follow the guide for [Setting up Amazon SageMaker Unified Studio](https://docs.aws.amazon.com/sagemaker-unified-studio/latest/adminguide/setting-up.html)).
* Grant Lambda access within SageMaker Unified Studio (required to Publish assets)
  + Add the Lambda execution role as an IAM role in SageMaker Unified Studio.
  + Assign the Lambda execution role to your project in the SageMaker Unified Studio console.

This configuration ensures the Lambda function has the permissions needed to access Data Zone resources and publish assets successfully from SageMaker Unified Studio projects.

## Deploying the code

Review the instructions in our [GitHub repository](https://github.com/aws-samples/sample-sagemaker-unified-studio-governance-notifications) to deploy the framework into your AWS account using the AWS CDK. CDK provides an event‑driven notification architecture for Amazon SageMaker Unified Studio, focused on project creation and asset publication events.

**Core AWS resources deployed** – The following core AWS resources are deployed:

1. **EventBridge rules**
   * DataZoneCreateProjectRule: captures DataZone create project events (`CreateProject`).
   * DataZonePublishAssetRule: captures DataZone publish asset events (`CreateListingChangeSet` with action `PUBLISH` for entity type `ASSET`).

2. **SQS queues**
   * DataZoneEventQueue: buffers DataZone events from EventBridge prior to processing.
   * Queue policy: allows EventBridge to send messages to the SQS queue.

3. **Lambda functions**
   * ProjectNotificationLambda: processes messages from the SQS queue, retrieves event details from DataZone, and sends notifications to SNS.
     + IAM role: grants access to SQS, SNS, CloudWatch Logs, and DataZone APIs.
     + Event source mapping: triggers the Lambda for each SQS message.

4. **SNS topic**
   * LambdaSNSTopic: receives notifications from Lambda.
     + Email subscriptions: add your email addresses to the SNS topic. You will receive a confirmation email — click "Confirm Subscription" to activate.

5. **Permissions**
   * EventBridge must be allowed to send events to SQS (SQS permissions), Lambda must be allowed to read from SQS (Lambda execution role), and Lambda must be allowed to publish to SNS (SNS permissions).
   * IAM policies: the Lambda execution role includes the necessary permissions for SQS, SNS, logging, and DataZone operations.

## Outputs (CloudFormation outputs)

* SNS topic ARN: for publishing notifications.
* SQS queue ARN: for event buffering.
* Lambda function ARN: for event processing.
* EventBridge rule ARNs: for project creation and asset publication events.

## Project creation notifications

Follow these steps to sign in to SageMaker Unified Studio and create a project.

1. Sign in to the SageMaker Unified Studio console. This will take you to the Amazon SageMaker Unified Studio domain sign-in screen (SSO and IAM options).

   ![SageMaker Unified Studio sign-in](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-2.jpg)

2. Choose **Create Project** on the SageMaker Unified Studio landing page.

   ![Create project](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-3.jpg)

3. Enter a project name such as `My_Demo_Project`. For the project profile, select `All-Capabilities`.

   ![Demo project](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-4.jpg)

4. Choose **Continue** and keep the defaults.

   ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-5.jpg)

5. Choose **Continue** then **Create project** on the next screen.

   ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-6.jpg)

6. The final screen displays the created project.

   ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-7.jpg)

7. Email notification: after the project is successfully created, an email notification is sent by the automation deployed above.

   ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-8.jpg)

## Asset publish notifications

To publish a sample asset in SageMaker Unified Studio:

1. **Lambda permissions**

   After the CDK stack creates the Lambda execution role `DatazoneStack-LambdaExecutionRole`, associate that role with your SageMaker Studio project so the Lambda can interact with the DataZone API and publish assets from the project.

   1. Sign in to SageMaker Unified Studio via SSO, select Members, then Add members.
   2. Find the role `DatazoneStack-LambdaExecutionRole` and add it with the role `Contributor`.

      ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-9.png)

      ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-10.png)

   The Lambda execution role (`<cf-stack-name>-LambdaExecutionRole`) is added as a member to a project in SageMaker Unified Studio.

2. **Create an asset**
   1. In your `My_Demo_Project`, choose Data and click the plus icon to add a dataset.

      ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-11.jpg)

   2. Upload your CSV file using the `Product_v6.csv` sample available in the repository checkout `sample-sagemaker-unified-studio-governance-notifications`.

      ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-12.jpg)

   3. Select table type S3/external table.

      ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-13.jpg)

   4. Review and confirm that the column/attribute names in the uploaded CSV are correct.

      ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-14.jpg)

   5. Check the Glue database (**glue_db_<unique_id>**) to confirm the table was created and populated correctly.

3. **Publish an asset**
   1. Select the asset, then choose Actions → **Publish to Catalog**.

      ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-15.jpg)

      ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-16.jpg)

   2. View the published asset.

      ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-17.jpg)

   3. In the Project Catalog's Assets section, locate the item and verify the published table name.

      ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-18.jpg)

   4. Choose the asset name to view additional details and attributes about the table/asset.

4. **Email alerts**
   1. After an asset is published to SageMaker Unified Studio, an email alert will be sent with the asset details. Central governance teams can use these alerts to review published assets and ensure they meet enterprise standards.

      ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-19.jpg)

      The email alert is sent to notify stakeholders when an asset has been published.

## Cleanup

To remove your resources, run:

```bash
cdk destroy --profile <PIPELINE-PROFILE>
```

## Conclusion

In this post, you learned how to build an automated notification system for Amazon SageMaker Unified Studio using AWS services. Specifically, we covered:

* How to set up event-driven notifications from Amazon SageMaker Unified Studio using Amazon EventBridge, AWS Lambda, and Amazon SNS.
* Step-by-step deployment using AWS CDK.
* Real-world examples for monitoring important events such as project creation and asset publication.
* How to integrate Lambda permissions with SageMaker Unified Studio for secure operations.
* Best practices for implementing governance controls through automated notifications.

Amazon SageMaker Catalog helps governance teams stay informed of catalog activity in near real time, enabling them to maintain organizational standards as their data and ML platforms scale. The architecture is flexible and extensible to integrate with enterprise workflow tools (e.g., ServiceNow) or to monitor additional event types as your organization requires.

We look forward to hearing how you adapt this solution for your governance needs. Fork our CDK repo and share your deployment experience in the comments below.

---

## About the authors

### Himanshu Sahni

[Himanshu](https://www.linkedin.com/in/himanshu-sahni-68b3281b/) is a Senior Data and AI Architect in AWS Professional Services. Himanshu builds data and analytics solutions for enterprise customers using AWS tools and services. He specializes in AI/ML and big data technologies such as Spark, AWS Glue, and Amazon EMR. Outside work, Himanshu enjoys chess and tennis.

### Rajiv Upadhyay

[Rajiv](https://www.linkedin.com/in/rajiv-upadhyay1/) is a Data Architect at AWS, building data and analytics solutions for enterprise customers. He guides organizations through digital transformation with expertise in data lakes, data governance, and AI/ML solutions.

### Jitesh Kumar

[Jitesh](https://www.linkedin.com/in/jitesh-kumar-23b510a/) is a Senior Customer Solutions Manager at Amazon Web Services (AWS), helping organizations realize the full potential of cloud technologies. Passionate about driving technical innovation, Jitesh combines deep technical knowledge with customer-first thinking to guide businesses through cloud transformations and deliver measurable outcomes.

