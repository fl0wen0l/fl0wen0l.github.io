---
title: "Week 5 Worklog"
date: "2025-08-30"
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---



### Week 5 Objectives (AWS Cloud9 & Amazon S3)

This week covers using AWS Cloud9 as a cloud IDE for development and how to host static websites using Amazon S3. You will learn to create Cloud9 environments, use the built-in terminal/CLI, and deploy static content to S3 with best practices for public access and acceleration (CloudFront).

* Create and use AWS Cloud9 environments for development and AWS CLI operations.
* Host a static website on Amazon S3: enable static hosting, configure public objects, and verify public access.
* Accelerate and secure static hosting using Amazon CloudFront and S3 best practices (versioning, replication, and public access block).
* Clean up resources and apply cost/cultural best practices.

### Tasks to be carried out this week (aligned with https://000049.awsstudygroup.com/ and https://000057.awsstudygroup.com/):
| Day | Task                                                                                                                                                                                                | Start Date | Completion Date | Reference Material                                  |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | --------------------------------------------------- |
| 1   | Module 1 (Cloud9) — Create Cloud9 environment: Create an AWS Cloud9 instance and explore the IDE, settings, and preferences.                                                                        | 08/30/2025 | 08/30/2025      | <https://000049.awsstudygroup.com/1-createcloud9/>  |
| 2   | Module 2 (Cloud9) — Cloud9 basic operations: Use the terminal, code editor, debugging, and the AWS CLI inside Cloud9 to manage resources.                                                           | 08/31/2025 | 08/31/2025      | <https://000049.awsstudygroup.com/2-basicfeature/>  |
| 3   | Module 3 (Cloud9) — Using AWS CLI: Practice using AWS CLI from Cloud9 (deploy, navigate, troubleshoot), and set up IAM roles/credentials as needed for the environment.                             | 09/01/2025 | 09/01/2025      | <https://000049.awsstudygroup.com/3-useawscli/>     |
| 4   | Module 1 (S3) — Prepare S3 and enable static website hosting: Create a bucket, enable static hosting, and set index/error documents.                                                                | 09/02/2025 | 09/02/2025      | <https://000057.awsstudygroup.com/1-introduce/>     |
| 5   | Module 3 (S3) — Configure public access and objects: Configure the public access block and set object ACLs or bucket policies to allow public sites, and upload a sample static site (HTML/CSS/JS). | 09/03/2025 | 09/03/2025      | <https://000057.awsstudygroup.com/3-staticwebsite/> |
| 6   | Module 7 & 8 (S3) — Acceleration & Versioning: Setup CloudFront to accelerate the S3 site; enable bucket versioning and consider lifecycle/replication options if required.                         | 09/04/2025 | 09/04/2025      | <https://000057.awsstudygroup.com/7-cloudfront/>    |
| 7   | Module 11 — Test & Cleanup: Verify the website is accessible, automate uploads via Cloud9/CLI, and cleanup resources (Cloud9 environment, S3 buckets, CloudFront distributions).                    | 09/05/2025 | 09/05/2025      | <https://000057.awsstudygroup.com/6-testwebsite/>   |


### Week 5 Achievements (aligned with Cloud9 & S3 workshop content):

* Created and explored an AWS Cloud9 environment; used the built-in editor and terminal for development and AWS CLI operations.
* Performed Cloud9 tasks: configured environment settings, used the debugger, and ran sample scripts from the Cloud9 terminal.
* Created an S3 bucket and enabled static website hosting; uploaded sample web content and verified website availability.
* Configured public access (bucket policy or ACL), tested object-level access, and set up CloudFront distribution for performance.
* Implemented versioning for the S3 bucket and reviewed cleanup steps to delete Cloud9 environments, buckets, and CloudFront resources to avoid charges.
* Automated uploads using AWS CLI from Cloud9, demonstrating an integrated development and deployment workflow.
### Quick Reference / Commands (Cloud9 & S3)

```bash
# Cloud9: Create an environment in Console or via CloudFormation/CLI if supported
# Using AWS CLI inside Cloud9 to upload site
aws s3 mb s3://my-static-site-bucket --region us-east-1
aws s3 website s3://my-static-site-bucket --index-document index.html --error-document error.html
aws s3 cp ./site s3://my-static-site-bucket/ --recursive --acl public-read

# CloudFront distribution creation snippet (example)
aws cloudfront create-distribution --origin-domain-name my-static-site-bucket.s3.amazonaws.com

# Enable versioning
aws s3api put-bucket-versioning --bucket my-static-site-bucket --versioning-configuration Status=Enabled

# Cleanup example
aws s3 rb s3://my-static-site-bucket --force
```

### References:

- AWS Cloud9: <https://000049.awsstudygroup.com/>
- Static Website Hosting (S3): <https://000057.awsstudygroup.com/>


