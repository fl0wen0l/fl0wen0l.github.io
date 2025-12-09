---
title: "Clean Up Resources"
date: "`r Sys.Date()`"
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

#### Why is cleanup important?

In a Cloud environment, you pay for what you provision. Even though Serverless services like Lambda and DynamoDB (On-Demand) have a generous Free Tier or low idle costs, other resources might incur charges over time:

- **S3 Storage**: You pay for the data stored in buckets
- **Amazon Location Service**: Storing Place Indexes or using Maps
- **CloudWatch Logs**: Stored log data

To prevent unexpected billing, always clean up your environment after completing a workshop.

#### Content

- [Destroy the Stack](5.6.1-destroy-stack/)
- [Verify Resource Deletion](5.6.2-verify-deletion/)
- [Workshop Conclusion](5.6.3-conclusion/)
