---
title: "Week 6 Worklog"
date: "2025-09-06"
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---



### Week 6 Objectives (Amazon RDS)

This week covers Amazon Relational Database Service (RDS): creating managed database instances, selecting the appropriate engine and storage, setting up availability, backups and security, and deploying an application that uses RDS. You'll learn to perform hands-on RDS tasks, operate backups/restores, and understand scaling, monitoring, and cleanup.

* Understand RDS capabilities and supported engines (Aurora, MySQL, MariaDB, SQL Server, PostgreSQL, Oracle).
* Configure storage options (gp3, io2) and high-availability (Multi-AZ, Read Replicas).
* Launch RDS instances, configure database and security settings, and connect from EC2.
* Deploy a sample application that uses RDS and practice backups and restore operations.

### Tasks to be carried out this week (aligned with https://000005.awsstudygroup.com/):
| Day | Task                                                                                                                                                                                   | Start Date | Completion Date | Reference Material                                |
| --- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ------------------------------------------------- |
| 1   | Module 1 — Introduction & Preparation: Read the RDS overview, supported engines, and identify the DB engine to use. Prepare VPC, subnet, and Security Group rules for DB access.       | 09/06/2025 | 09/06/2025      | <https://000005.awsstudygroup.com/1-introduce/>   |
| 2   | Module 2 — Prepare prerequisites: Launch or prepare EC2 instances as application hosts (if required), configure IAM roles and create RDS subnet groups and parameter groups.           | 09/07/2025 | 09/07/2025      | <https://000005.awsstudygroup.com/2-prerequiste/> |
| 3   | Module 3 — Create EC2 instance(s): Launch an EC2 instance to connect and test DB access (if your app is deployed on EC2). Configure networking and security for connectivity.          | 09/08/2025 | 09/08/2025      | <https://000005.awsstudygroup.com/3-create-ec2/>  |
| 4   | Module 4 — Create RDS database instance: Create RDS instance (engine choice), storage type, Multi-AZ or Read Replica options, and networking settings; validate connectivity from EC2. | 09/09/2025 | 09/09/2025      | <https://000005.awsstudygroup.com/4-create-rds/>  |
| 5   | Module 5 — Application Deployment: Deploy sample app (e.g., user management or web app) to EC2 and configure it to connect to the RDS instance (DB endpoint, credentials).             | 09/10/2025 | 09/10/2025      | <https://000005.awsstudygroup.com/5-deploy-app/>  |
| 6   | Module 6 — Backup and Restore: Practice automated backups, create manual snapshots, test point-in-time recovery and manual snapshot restore into new instances for validation.         | 09/11/2025 | 09/11/2025      | <https://000005.awsstudygroup.com/6-backup/>      |
| 7   | Module 7 — Clean up and documentation: Terminate instances, remove DB instances appropriately (snapshot if necessary), and document steps and observations.                            | 09/12/2025 | 09/12/2025      | <https://000005.awsstudygroup.com/7-cleanup/>     |


### Week 6 Achievements (aligned with https://000005.awsstudygroup.com/):

* Analysed and selected an appropriate DB engine for the sample application (e.g., MySQL/PostgreSQL/Aurora) and selected storage type (gp3/io2) based on performance needs.
* Created subnet groups and parameter groups to support the RDS deployment and configured secure network access using Security Groups and VPC settings.
* Launched RDS instances and validated application connectivity from EC2; observed typical DB behavior and identified best-ops metrics.
* Implemented automated backups and created manual snapshots; successfully tested restore and recovery procedures into a new DB instance.
* Deployed a sample app that uses RDS for data persistence and validated basic CRUD operations.
* Completed resource cleanup and documented all steps, configuration choices, and lessons learned for reproducibility and cost control.

---

### Quick Reference / Commands (RDS & Example)

```bash
# Create a DB Subnet Group
aws rds create-db-subnet-group --db-subnet-group-name my-subnet-group --db-subnet-group-description "week6-subnet-group" --subnet-ids subnet-xxxx subnet-yyyy

# Create a MySQL RDS instance (example)
aws rds create-db-instance --db-instance-identifier mydbinstance --db-instance-class db.t3.micro --engine mysql --master-username admin --master-user-password "ReplaceWithSecurePass!" --allocated-storage 20 --db-subnet-group-name my-subnet-group --vpc-security-group-ids sg-xxxx

# Create a snapshot
aws rds create-db-snapshot --db-instance-identifier mydbinstance --db-snapshot-identifier mydbsnapshot

# Restore from snapshot (into a new instance)
aws rds restore-db-instance-from-db-snapshot --db-instance-identifier mydb-restored --db-snapshot-identifier mydbsnapshot

# Delete DB instance (finalize cleanup)
aws rds delete-db-instance --db-instance-identifier mydbinstance --skip-final-snapshot
```

### References:

- Amazon RDS overview & modules: <https://000005.awsstudygroup.com/>


