---
title: "Week 8 Worklog"
date: "2025-09-20"
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---




### Overview

Week 8 covers deploying the FCJ Management application using Amazon EC2 Auto Scaling Groups and an Application Load Balancer. The goal is to build a resilient, scalable architecture that adjusts capacity automatically to meet demand. This worklog follows the workshop at https://000006.awsstudygroup.com/.

### Objectives

- Understand Auto Scaling Group concepts and how they integrate with Launch Templates and Load Balancers.
- Create a Launch Template, provision an Application Load Balancer, and attach instances to the ALB.
- Configure and test an Auto Scaling Group (ASG) with scaling policies.
- Validate application behavior under load and perform cleanup to avoid charges.

### Prerequisites

- AWS account with permissions to create EC2, EC2 Auto Scaling, and Elastic Load Balancing resources.
- Basic knowledge of EC2 instance creation and AMIs (see Week 3 content for reference).
- Optional: `awscli` configured for quick testing from terminal.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                | Start Date | Completion Date | Reference Material                                                                                    |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------------------------------------------------------------------- |
| 1   | Workshop overview & preparation: read the guide and confirm account permissions and quotas.                                                         | 09/20/2025 | 09/20/2025      | <https://000006.awsstudygroup.com/>                                                                   |
| 2   | Module 1 — Introduction & Preparation: gather AMI/instance details, security groups, and required roles.                                            | 09/21/2025 | 09/21/2025      | <https://000006.awsstudygroup.com/1-introduction/>, <https://000006.awsstudygroup.com/2-preparation/> |
| 3   | Module 2 — Create Launch Template: create and test a Launch Template (AMI, instance type, user data, key pair, security group).                     | 09/22/2025 | 09/22/2025      | <https://000006.awsstudygroup.com/3-create-launch-template/>                                          |
| 4   | Module 3 — Setup Load Balancer: create an Application Load Balancer, target group, and register targets.                                            | 09/23/2025 | 09/23/2025      | <https://000006.awsstudygroup.com/4-setup-load-balancer/>                                             |
| 5   | Module 4 — Create Auto Scaling Group: create ASG using the Launch Template and attach it to the target group; configure min/max/desired capacities. | 09/24/2025 | 09/24/2025      | <https://000006.awsstudygroup.com/6-create-auto-scaling-group/>                                       |
| 6   | Module 5–6 — Test & Test Solutions: simulate load, verify scaling actions, and validate health checks and replacement behavior.                     | 09/25/2025 | 09/25/2025      | <https://000006.awsstudygroup.com/5-test/>, <https://000006.awsstudygroup.com/7-test-solutions/>      |
| 7   | Module 7 — Cleanup: terminate ASG, delete Launch Template, ALB, target groups, and any leftover resources.                                          | 09/26/2025 | 09/26/2025      | <https://000006.awsstudygroup.com/8-cleanup/>                                                         |

### Week 8 Achievements

- Built a Launch Template and verified instance bootstrapping (user-data) works as expected.
- Provisioned an Application Load Balancer and registered application instances.
- Created and validated an Auto Scaling Group with expected scaling behavior under test load.
- Confirmed health checks and automatic replacement of unhealthy instances.
- Cleaned up all resources to avoid residual costs.

### Quick Reference (CLI examples)

Note: replace placeholder names with actual values from your account.

```bash
# Create a launch template
aws ec2 create-launch-template --launch-template-name my-launch-template --version-description v1 --launch-template-data '{"ImageId":"ami-0123456789abcdef0","InstanceType":"t3.micro","UserData":"$(echo -n \"#!/bin/bash\n...\" | base64)"}'

# Create a target group
aws elbv2 create-target-group --name my-targets --protocol HTTP --port 80 --vpc-id vpc-xxxxxxxx

# Create an application load balancer
aws elbv2 create-load-balancer --name my-alb --subnets subnet-aaa subnet-bbb --security-groups sg-xxxxxx

# Register targets (example)
aws elbv2 register-targets --target-group-arn arn:aws:elasticloadbalancing:...:targetgroup/my-targets/xxxxxxxx --targets Id=i-0123456789abcdef0

# Create an autoscaling group
aws autoscaling create-auto-scaling-group --auto-scaling-group-name my-asg --launch-template LaunchTemplateName=my-launch-template,Version=1 --min-size 1 --max-size 3 --desired-capacity 1 --vpc-zone-identifier "subnet-aaa,subnet-bbb" --target-group-arns arn:aws:elasticloadbalancing:...:targetgroup/my-targets/xxxxxxxx

# Delete autoscaling group (cleanup)
aws autoscaling delete-auto-scaling-group --auto-scaling-group-name my-asg --force-delete

# Delete launch template
aws ec2 delete-launch-template --launch-template-name my-launch-template
```

### References

- Deploying FCJ Management Application with Auto Scaling Group: <https://000006.awsstudygroup.com/>
- Modules: Introduction, Preparation, Create Launch Template, Setup Load Balancer, Test, Create Auto Scaling Group, Test Solutions, Cleanup

---

If you'd like, I can:
- Add detailed per-day CLI step-by-step instructions.
- Generate IaC (CloudFormation/Terraform) snippets for Launch Template/ASG/ALB (note: test locally; do not run in your account without review).
- Produce a Vietnamese translation of this English content.

