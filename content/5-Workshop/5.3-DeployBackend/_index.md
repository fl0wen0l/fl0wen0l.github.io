---
title: "Deploy Backend"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

#### Introduction to Infrastructure as Code (IaC)

Instead of manually clicking through the AWS Console ("ClickOps"), we use **AWS CDK** to define our entire infrastructure. In the file `cdk/lib/backend-stack.ts`, we have designed a complete Serverless system.

When you run the `cdk deploy` command, CDK synthesizes this code into a **CloudFormation Template**, and AWS automatically provisions the corresponding resources.

![Architecture](/images/5-Workshop/5.3-DeployBackend/architecture.png)

#### Content

- [Architecture Deep Dive](5.3.1-architecture/)
- [Install Dependencies](5.3.2-install-dependencies/)
- [Deploy Stack](5.3.3-deploy-stack/)
- [Results & Outputs](5.3.4-results/)
