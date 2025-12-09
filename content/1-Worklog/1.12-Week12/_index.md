---
title: "Week 12 Worklog"
date: "2025-11-01"
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---




### Overview

Week 12 is focused on deploying WordPress on AWS Cloud using EC2, RDS, Load Balancers, Auto Scaling, and CloudFront. The content follows the workshop "TRIỂN KHAI WORDPRESS TRÊN AWS CLOUD" at https://000021.awsstudygroup.com/.

### Objectives

- Deploy a highly available WordPress site using EC2 instances in public subnets and an RDS database in private subnets.
- Configure an Application Load Balancer, Target Group, and Auto Scaling Group for the web tier.
- Use Amazon RDS (Multi‑AZ) for database availability and implement snapshot/backup and restore procedures.
- (Optional) Configure Amazon CloudFront as CDN for improved global performance.
- Perform cleanup to remove resources and avoid charges.

### Prerequisites

- AWS account with permissions for EC2, RDS, ELB, Auto Scaling, CloudFront, and IAM.
- Basic knowledge of VPC, subnets, security groups, and SSH access to EC2 instances.
- `awscli` configured (optional) for running CLI examples.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                   | Start Date | Completion Date | Reference Material                                                                                 |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | -------------------------------------------------------------------------------------------------- |
| 1   | Workshop overview & environment prep: read guide, confirm VPC/subnet strategy and IAM roles.                                                           | 11/01/2025 | 11/01/2025      | <https://000021.awsstudygroup.com/1-introduce/>, <https://000021.awsstudygroup.com/2-prerequiste/> |
| 2   | Install WordPress on EC2: launch EC2 in public subnet, install web stack (NGINX/Apache + PHP), and configure WordPress files.                          | 11/02/2025 | 11/02/2025      | <https://000021.awsstudygroup.com/3-installwordpressonec2/>                                        |
| 3   | RDS setup: create an RDS instance in private subnet (Multi‑AZ optional), configure security groups, and connect WordPress to RDS.                      | 11/03/2025 | 11/03/2025      | <https://000021.awsstudygroup.com/5-backupandrestore/>                                             |
| 4   | Auto Scaling & Load Balancer: create Target Group, ALB, and Auto Scaling Group for web tier; verify session handling and health checks.                | 11/04/2025 | 11/04/2025      | <https://000021.awsstudygroup.com/4-asgforec2/>                                                    |
| 5   | Snapshot & Backup: create RDS snapshots, test restore procedure, and implement instance snapshots for web tier.                                        | 11/05/2025 | 11/05/2025      | <https://000021.awsstudygroup.com/5-backupandrestore/>                                             |
| 6   | CloudFront (optional): create CloudFront distribution for the WordPress site, configure origin, and test cached responses.                             | 11/06/2025 | 11/06/2025      | <https://000021.awsstudygroup.com/6-createcloudfront/>                                             |
| 7   | Cleanup: delete EC2 instances, Auto Scaling Group, Target Group, ALB, RDS instances/snapshots (if test), CloudFront distribution, and other artifacts. | 11/07/2025 | 11/07/2025      | <https://000021.awsstudygroup.com/7-cleanup/>                                                      |

### Week 12 Achievements

- Deployed a WordPress site on EC2 connected to an RDS backend and verified end-to-end functionality.
- Configured an ALB and Auto Scaling Group to provide resilience and scaling for the web tier.
- Created and restored RDS snapshots to validate backup/restore procedures.
- (Optional) Deployed CloudFront distribution and observed improved content delivery.
- Cleaned up resources to avoid ongoing charges.

### Quick Reference (CLI examples)

Note: replace placeholder values with those from your account.

```bash
# Launch an EC2 instance (example)
aws ec2 run-instances --image-id ami-0123456789abcdef0 --count 1 --instance-type t3.micro --key-name MyKey --security-group-ids sg-xxxx --subnet-id subnet-aaa

# Create an RDS instance (MySQL example)
aws rds create-db-instance --db-instance-identifier wordpress-db --allocated-storage 20 --db-instance-class db.t3.micro --engine mysql --master-username admin --master-user-password 'P@ssw0rd!' --vpc-security-group-ids sg-xxxx --db-subnet-group-name my-db-subnet-group --multi-az

# Create an application load balancer
aws elbv2 create-load-balancer --name wordpress-alb --subnets subnet-aaa subnet-bbb --security-groups sg-xxxx

# Create target group
aws elbv2 create-target-group --name wordpress-targets --protocol HTTP --port 80 --vpc-id vpc-xxxxxxxx

# Register instance to target group
aws elbv2 register-targets --target-group-arn arn:aws:elasticloadbalancing:...:targetgroup/wordpress-targets/xxxxxxxx --targets Id=i-0123456789abcdef0

# Create Auto Scaling Group (example)
aws autoscaling create-auto-scaling-group --auto-scaling-group-name wordpress-asg --launch-configuration-name my-launch-config --min-size 1 --max-size 3 --desired-capacity 1 --vpc-zone-identifier "subnet-aaa,subnet-bbb"

# Create RDS snapshot
aws rds create-db-snapshot --db-instance-identifier wordpress-db --db-snapshot-identifier wordpress-db-snap-1

# Create CloudFront distribution (example using S3 origin or ALB)
aws cloudfront create-distribution --distribution-config file://cf-distribution.json

# Cleanup examples (delete resources when finished)
aws rds delete-db-instance --db-instance-identifier wordpress-db --skip-final-snapshot --delete-automated-backups
aws elbv2 delete-load-balancer --load-balancer-arn arn:aws:elasticloadbalancing:...:loadbalancer/app/wordpress-alb/xxxxxxxx
aws autoscaling delete-auto-scaling-group --auto-scaling-group-name wordpress-asg --force-delete
```

### References

- Deploy WordPress on AWS Cloud: <https://000021.awsstudygroup.com/>
- Modules: Introduction, Preparatory Steps, Install WordPress on EC2, Auto Scaling for WordPress, Backup and Restore, CloudFront, Cleanup

---

If you'd like, I can:
- Expand each day with precise step-by-step commands and verification checks.
- Provide a sample `cf-distribution.json` and `dashboard.json` for CloudFront and monitoring.
- Produce a Vietnamese translation of the page.


