---
title: "Week 10 Worklog"
date: "2025-10-11"
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---




### Overview

Week 10 covers two complementary workshops:
- "SET UP HYBRID DNS WITH ROUTE 53 RESOLVER" (https://000010.awsstudygroup.com/) — design and implement a hybrid DNS architecture using Route 53 Resolver inbound/outbound endpoints and rules.
- "GETTING STARTED WITH THE AWS CLI" (https://000011.awsstudygroup.com/) — install, configure, and use the AWS CLI across common services.

Combining both topics gives practical skills: manage infrastructure via the CLI and integrate on-prem DNS with AWS for hybrid scenarios.

### Objectives

- Install and configure the AWS CLI and basic profiles.
- Use the CLI to inspect and manage AWS resources (S3, EC2, IAM, VPC, Route 53).
- Deploy Route 53 Resolver inbound/outbound endpoints and Resolver rules to enable DNS forwarding between AWS and on-prem systems.
- Validate DNS resolution flow and clean up resources after testing.

### Prerequisites

- AWS account with permissions for Route 53 Resolver, EC2, VPC, IAM, and basic services used in CLI examples.
- Local environment with AWS CLI installed (or use CloudShell).
- If testing the hybrid DNS flow, an on‑prem or jump host that can reach the Resolver endpoints (RDGW/remote host). See the workshop for network topology details.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                       | Start Date | Completion Date | Reference Material                                                                                       |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | -------------------------------------------------------------------------------------------------------- |
| 1   | Workshop overview & environment preparation: read both guides, confirm IAM permissions, and set CLI profile(s).                            | 10/11/2025 | 10/11/2025      | <https://000010.awsstudygroup.com/>, <https://000011.awsstudygroup.com/>                                 |
| 2   | AWS CLI: install/upgrade AWS CLI v2, configure default and named profiles, practice `aws configure`, `aws sts get-caller-identity`.        | 10/12/2025 | 10/12/2025      | <https://000011.awsstudygroup.com/3-installcli/>, <https://000011.awsstudygroup.com/1-introduce/>        |
| 3   | AWS CLI hands-on: explore S3, SNS, IAM, VPC and EC2 via CLI (`aws s3 ls`, `aws ec2 describe-instances`, `aws iam list-roles`).             | 10/13/2025 | 10/13/2025      | <https://000011.awsstudygroup.com/4-infras/>, <https://000011.awsstudygroup.com/5-s3/>                   |
| 4   | Route 53 Resolver: prepare network (VPC/subnets), create inbound/outbound endpoints, and configure Resolver rules.                         | 10/14/2025 | 10/14/2025      | <https://000010.awsstudygroup.com/2-prerequiste/>, <https://000010.awsstudygroup.com/5-setuphyriddns/>   |
| 5   | Connect to RDGW / test DNS: verify on‑prem to AWS resolution and AWS to on‑prem forwarding; troubleshoot using `dig`/`nslookup`.           | 10/15/2025 | 10/15/2025      | <https://000010.awsstudygroup.com/3-connecttordgw/>, <https://000010.awsstudygroup.com/5-setuphyriddns/> |
| 6   | Validate and automate: use AWS CLI to list Resolver endpoints/rules, and test scripted flows; capture test results.                        | 10/16/2025 | 10/16/2025      | <https://000010.awsstudygroup.com/> , <https://000011.awsstudygroup.com/>                                |
| 7   | Cleanup: remove Resolver endpoints and rules, remove any test resources, and ensure IAM keys/profiles are rotated or deleted if temporary. | 10/17/2025 | 10/17/2025      | <https://000010.awsstudygroup.com/6-cleanup/>, <https://000011.awsstudygroup.com/11-cleanup/>            |

### Week 10 Achievements

- Installed and configured AWS CLI profiles and validated access with `aws sts get-caller-identity`.
- Used CLI to inspect S3, EC2, IAM, VPC resources and automate small tasks.
- Deployed Route 53 Resolver inbound/outbound endpoints and resolver rules to forward DNS between on‑prem and AWS.
- Validated DNS queries flow between on‑prem and AWS and documented troubleshooting steps.
- Performed cleanup to remove test resources and minimize charges.

### Quick Reference (CLI examples)

Replace placeholders with values from your account and region.

```bash
# Configure AWS CLI (interactive)
aws configure --profile fcj

# Verify identity
aws sts get-caller-identity --profile fcj

# List S3 buckets
aws s3 ls --profile fcj

# Describe EC2 instances
aws ec2 describe-instances --profile fcj

# Create a Route 53 Resolver outbound endpoint (example)
aws route53resolver create-resolver-endpoint --creator-request-id "fcj-outbound-1" --name "fcj-outbound" --direction OUTBOUND --ip-addresses SubnetId=subnet-aaa,Ip=10.0.1.10 SubnetId=subnet-bbb,Ip=10.0.2.10 --security-group-ids sg-xxxx --profile fcj

# Create a Route 53 Resolver rule to forward specific domain to on-prem DNS
aws route53resolver create-resolver-rule --creator-request-id "fcj-rule-1" --name "forward-onprem" --rule-type FORWARD --domain-name "corp.example.local" --target-ips Ip=10.0.100.10,Port=53 --profile fcj

# Associate resolver rule with a VPC
aws route53resolver associate-resolver-rule --resolver-rule-id rslvr-rr-xxxxxxxx --vpc-id vpc-xxxxxxxx --profile fcj

# List resolver endpoints and rules
aws route53resolver list-resolver-endpoints --profile fcj
aws route53resolver list-resolver-rules --profile fcj

# Cleanup (delete resolver rule and endpoint)
aws route53resolver delete-resolver-rule --resolver-rule-id rslvr-rr-xxxxxxxx --profile fcj
aws route53resolver delete-resolver-endpoint --resolver-endpoint-id rslvr-out-xxxxxxxx --profile fcj
```

### References

- Set up Hybrid DNS with Route 53 Resolver: <https://000010.awsstudygroup.com/>
- Getting Started with the AWS CLI: <https://000011.awsstudygroup.com/>

---

If you'd like, I can:
- Provide step-by-step CLI commands for each day (safe, non-destructive examples).
- Generate small IaC snippets (CloudFormation/Terraform) for creating Resolver endpoints (review before running).
- Produce a Vietnamese translation of this content.


