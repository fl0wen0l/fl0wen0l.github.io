---
title: "Introduction"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

#### Serverless Architecture

**Serverless architecture** allows you to build and run applications and services without thinking about servers. It eliminates infrastructure management tasks such as server provisioning, patching, operating system maintenance, and capacity provisioning.
The architecture relies on **AWS Lambda** for compute, **Amazon API Gateway** for API management, and **Amazon DynamoDB** for data storage. Compute resources running in Lambda are triggered by events from API Gateway and scale automatically.

#### Workshop overview

In this workshop, you will deploy the Findnest Backend using AWS CDK.
"**Backend Logic**" is handled by **AWS Lambda** running Node.js/Express code. It serves as the "brain" of the application, processing business logic only when triggered.
"**Data & Storage**" utilizes **Amazon DynamoDB** for high-performance NoSQL data storage (Listings, Users) and **Amazon S3** for storing user-uploaded images securely. **Amazon API Gateway** acts as the secure entry point for all client requests.

![overview](/images/5-Workshop/5.1-Workshop-overview/diagram1.png)
