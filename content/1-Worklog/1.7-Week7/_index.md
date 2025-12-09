---
title: "Week 7 Worklog"
date: "2025-09-13"
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---




### Overview

Week 7 focuses on Amazon Lightsail — a simplified platform for deploying small web applications and managed databases. This week mirrors the "AMAZON LIGHTSAIL WORKSHOP - COST OPTIMIZATION ON AWS" (https://000045.awsstudygroup.com/).

### Objectives

- Get familiar with Amazon Lightsail and its available blueprints.
- Deploy open-source applications: WordPress, PrestaShop and Akaunting on Lightsail.
- Create and manage Lightsail relational databases, snapshots, and backups.
- Apply basic application security and cost-optimization best practices.
- Practice resource cleanup to avoid unintended charges.

### Prerequisites

- An AWS account with permissions to create Lightsail resources (Free Tier may apply).
- Optional: configured `awscli`, or access to the AWS Management Console.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                               | Start Date | Completion Date | Reference Material                                                                                                                                                                       |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Workshop overview & environment preparation: read the guide, check permissions, and set learning goals.                                                            | 09/13/2025 | 09/13/2025      | <https://000045.awsstudygroup.com/>                                                                                                                                                      |
| 2   | Module 1 — Deploy a Database on Lightsail: create a relational database instance, verify connectivity, and configure backups.                                      | 09/14/2025 | 09/14/2025      | <https://000045.awsstudygroup.com/1-database/>                                                                                                                                           |
| 3   | Module 2 — Deploying a WordPress Instance: use a Lightsail blueprint or OS-based setup to run WordPress and verify the site.                                       | 09/15/2025 | 09/15/2025      | <https://000045.awsstudygroup.com/2-wp-instance/>                                                                                                                                        |
| 4   | Module 3 — Deploying Prestashop E-Commerce Instance: deploy Prestashop using the Application Blueprint and test workflows.                                         | 09/16/2025 | 09/16/2025      | <https://000045.awsstudygroup.com/3-e-commerce-instance/>                                                                                                                                |
| 5   | Module 4 — Deploying Akaunting Instance: deploy Akaunting, ensure DB connectivity and basic configuration.                                                         | 09/17/2025 | 09/17/2025      | <https://000045.awsstudygroup.com/4-akaunting-instance/>                                                                                                                                 |
| 6   | Module 5–7 — Application Security, Snapshots, Scaling: apply security best practices (firewall rules), create snapshots, and practice scaling/upgrading instances. | 09/18/2025 | 09/18/2025      | <https://000045.awsstudygroup.com/5-secure-the-applications/>, <https://000045.awsstudygroup.com/6-create-snapshots/>, <https://000045.awsstudygroup.com/7-migrate-to-larger-instances/> |
| 7   | Module 8–9 — Alarms & Cleanup: create basic alarms, estimate cost impact, and perform resource cleanup following the guide.                                        | 09/19/2025 | 09/19/2025      | <https://000045.awsstudygroup.com/8-create-alarms/>, <https://000045.awsstudygroup.com/9-clean-up/>                                                                                      |

### Week 7 Achievements

- Successfully provision at least one Lightsail relational database and verify connectivity.
- Run WordPress, PrestaShop or Akaunting on Lightsail (at least one application end-to-end).
- Create instance snapshots and practice recovery (snapshot → new instance).
- Apply basic security controls (firewall rules, disable unnecessary services).
- Set up basic alarms for resource monitoring and perform cleanup to avoid charges.

### Quick Reference (CLI examples)

Note: replace `--instance-name`, `--relational-database-name` and `--blueprint-id` with real values.

```bash
# Create a Lightsail instance (example: WordPress blueprint)
aws lightsail create-instances --instance-names MyWordpress --availability-zone us-east-1a --blueprint-id wordpress --bundle-id micro_2_0

# Create a relational database
aws lightsail create-relational-database --relational-database-name my-db --availability-zone us-east-1a --master-username admin --master-user-password 'P@ssw0rd!'

# Create an instance snapshot
aws lightsail create-instance-snapshot --instance-name MyWordpress --instance-snapshot-name my-wordpress-snap

# Get instance information
aws lightsail get-instance --instance-name MyWordpress

# Delete instance (cleanup)
aws lightsail delete-instance --instance-name MyWordpress

# Delete relational database (cleanup)
aws lightsail delete-relational-database --relational-database-name my-db
```

### References

- Amazon Lightsail workshop: <https://000045.awsstudygroup.com/>
- Workshop modules: Deploy Database, WordPress, PrestaShop, Akaunting, Security, Snapshots, Scaling, Alarms, Cleanup

---


