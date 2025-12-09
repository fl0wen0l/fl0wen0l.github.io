---
title: "Building a Serverless Backend for FindNest (AWS CDK)"
date: "2025-12-02"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Workshop: Building a Serverless Backend for FindNest

Overview
--------

This hands‑on workshop guides participants through designing, implementing, and validating an end‑to‑end serverless backend for the FindNest application using AWS CDK (TypeScript). The workshop is modular: you can run the full lab or follow individual sections for architecture, deployment, data seeding, validation, and cleanup.

Learning objectives
-------------------
- Deploy serverless infrastructure with AWS CDK (TypeScript) in a reproducible manner
- Implement RESTful APIs using API Gateway and Lambda functions
- Model and store application data in Amazon DynamoDB
- Add user authentication and authorization using Amazon Cognito
- Integrate location services with Amazon Location Service and object storage in S3
- Integrate AI capabilities using Amazon Bedrock (Claude 3) where required
- Validate system behavior using test procedures; monitor with CloudWatch
- Apply secure IAM policies and clean up resources to avoid bills

Target audience
---------------
- Cloud engineers, backend developers, and DevOps practitioners who want practical experience with serverless application patterns on AWS.

Estimated time
--------------
- Full workshop: 2–4 hours (depending on environment and network speed)
- Per module: 15–90 minutes

Prerequisites
-------------
- An AWS account and administrative or developer credentials (with permission to create IAM roles, Lambda, API Gateway, DynamoDB, S3, SNS, CloudWatch, and related resources)
- A local development machine with:
  - Node.js 18+ (LTS recommended)
  - npm 8+ or yarn
  - AWS CLI configured with an AWS profile
  - AWS CDK 2.x installed
  - Git
- Recommended AWS Region: us-east-1 (adjust as necessary for service availability)
- Optional: An AWS Free Tier or test account to avoid impacting production resources

Architecture overview
---------------------
The workshop deploys a serverless architecture composed of:
- API Gateway for REST endpoints
- Lambda functions (Node.js) implementing backend logic
- Amazon DynamoDB for data storage (multiple tables for domain entities)
- Amazon Cognito for user authentication and authorization
- Amazon S3 for media storage (images)
- Amazon Location Service for map and geocoding features
- Amazon Bedrock to demonstrate AI integration workflows (Claude 3)
- Amazon SNS for optional notifications
- Amazon CloudWatch for monitoring and logging

Workshop modules and content
----------------------------
1. Workshop overview — `5.1-Workshop-overview/` (objectives, repo layout, and architecture reference)
2. Prerequisites — `5.2-Prerequiste/` (setup, CLI profile, tooling, and initial configuration)
3. Deploy backend — `5.3-DeployBackend/` (detailed submodules: architecture, install dependencies, deploy stack, results)
   - `5.3.1-architecture/` — design and sequence diagrams
   - `5.3.2-install-dependencies/` — local toolchain and build steps
   - `5.3.3-deploy-stack/` — CDK commands to bootstrap and deploy
   - `5.3.4-results/` — verify deployed resources and API endpoints
4. Seeding data — `5.4-SeedingData/` (prepare and run scripts to seed sample data and verify)
   - setup script, environment configuration, run script, verification
5. System validation — `5.5-SystemValidation/` (functional tests and smoke checks)
   - retrieve config, admin login test, create listing test, public access check, verify resources
6. Cleanup — `5.6-Cleanup/` (destroy stacks, verify resource deletion, and final notes)

Quick start (PowerShell example)
-------------------------------
After cloning the repository and setting up your CLI profile, use the following commands as examples to deploy the workshop in PowerShell:

```powershell
git clone https://github.com/your-org/findnest-workshop.git
cd findnest-workshop
npm ci
cdk bootstrap aws://<ACCOUNT_ID>/<REGION> --profile <PROFILE>
cdk deploy --all --profile <PROFILE>
```

Verification
------------
- Confirm the stacks are created in the chosen AWS account and region
- Check Lambda logs in CloudWatch and API responses via curl or Postman
- Verify DynamoDB tables created and seeded data exists if applicable

Cleanup
-------
To remove deployed resources and avoid charges, run:

```powershell
cdk destroy --all --profile <PROFILE>
```

Additional notes
----------------
- Replace placeholder values like `<PROFILE>`, `<ACCOUNT_ID>`, and `<REGION>` with appropriate values for your environment
- The workshop includes sample data and CloudFormation/CDK outputs; review them before use in a production account
- Monitor region availability and service limits; some services used (e.g., Bedrock) may be available in a limited set of regions

Repository and support
----------------------
- Repository (lab content): `https://github.com/your-org/findnest-workshop` (update to the actual repository URL)
- For issues or questions, contact the workshop owner or maintainers listed in the repository

If you want, I can:
- Add a step-by-step quick start checklist at the top for users who want a minimal path
- Add command examples for Linux/macOS and Windows PowerShell side-by-side
- Add a 'cost estimation' section and recommended AWS service quotas and limits
---
title: "Workshop"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---


# Building a Serverless Backend with AWS CDK

#### Overview

In this workshop, you will build a complete **Serverless Backend** for the **FindNest** application - a boarding house rental platform. You will use **AWS CDK (Cloud Development Kit)** to define and deploy your infrastructure as code.

You will learn how to:

- Deploy serverless infrastructure using AWS CDK with TypeScript
- Build RESTful APIs with API Gateway and Lambda
- Implement authentication with Amazon Cognito
- Store data in DynamoDB with proper data modeling
- Integrate AI capabilities with Amazon Bedrock (Claude 3)
- Add map functionality with Amazon Location Service
- Apply security best practices with IAM policies
- Manage infrastructure lifecycle (deploy and cleanup)

#### Architecture

You will deploy a modern serverless architecture consisting of:

- **API Gateway** - RESTful API endpoints
- **AWS Lambda** - Serverless compute (Node.js)
- **Amazon DynamoDB** - NoSQL database (7 tables)
- **Amazon Cognito** - User authentication and authorization
- **Amazon S3** - Image storage
- **Amazon Location Service** - Maps and geocoding
- **Amazon Bedrock** - AI/ML with Claude 3
- **Amazon SNS** - SMS notifications
- **CloudWatch Logs** - Monitoring and logging

#### Content

1. [Workshop Overview](5.1-Workshop-overview/)
2. [Prerequisites](5.2-Prerequiste/)
3. [Deploy Backend](5.3-DeployBackend/)
4. [Seeding Data](5.4-SeedingData/)
5. [System Validation](5.5-SystemValidation/)
6. [Clean Up Resources](5.6-Cleanup/)
