---
title: "Week 11 Worklog"
date: "2025-10-25"
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---




### Overview

Week 11 combines practical AWS CLI skills with migration planning and hands‑on migration patterns. It draws from:
- "GETTING STARTED WITH THE AWS CLI" (https://000011.awsstudygroup.com/) — to operate and automate AWS resources via the command line.
- "MIGRATE TO AWS" (https://cloudjourney.awsstudygroup.com/2-migrate/) — to understand migration strategies, assessment, and migration tools (VM import/export, DMS, etc.).

This week is about using the CLI to help assess, orchestrate, and validate migrations to AWS.

### Objectives

- Install and master basic AWS CLI workflows and profiles for automation.
- Learn migration planning concepts: assessment, lift-and-shift, replatforming, database migration (DMS), and validation.
- Use the CLI to collect inventory, run simple migration operations (VM import/export or DMS where applicable), and validate migrated resources.
- Build a checklist for safe migration and cleanup steps to avoid drift and excess cost.

### Prerequisites

- AWS account with permissions for EC2, S3, IAM, DMS (if available), and related services.
- Local environment with AWS CLI installed and configured profiles for the target account.
- Sample workload or VM artifacts for migration exercises (optional — use simulated artifacts if production data is not available).

### Tasks to be carried out this week:
| Day | Task                                                                                                                                        | Start Date | Completion Date | Reference Material                                                                                  |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | --------------------------------------------------------------------------------------------------- |
| 1   | Workshop overview & preparation: review both guides, confirm IAM permissions, set up CLI profiles, and identify workloads to assess.        | 10/25/2025 | 10/25/2025      | <https://000011.awsstudygroup.com/>, <https://cloudjourney.awsstudygroup.com/2-migrate/>            |
| 2   | CLI fundamentals: install/verify AWS CLI v2, create named profiles, practice common commands (`sts`, `s3`, `ec2`, `iam`).                   | 10/26/2025 | 10/26/2025      | <https://000011.awsstudygroup.com/3-installcli/>, <https://000011.awsstudygroup.com/1-introduce/>   |
| 3   | Inventory & assessment: use CLI to list instances, gather metadata, export inventories (CSV/JSON) for migration planning.                   | 10/27/2025 | 10/27/2025      | <https://000011.awsstudygroup.com/4-infras/>, <https://cloudjourney.awsstudygroup.com/2-migrate/>   |
| 4   | VM Import / Export & Storage: review VM import/export workflows or use S3 to stage artifacts; practice small import/export (dry-run).       | 10/28/2025 | 10/28/2025      | <https://cloudjourney.awsstudygroup.com/>, https://000014.awsstudygroup.com/                        |
| 5   | Database migration planning: overview DMS and SCT patterns; prepare source/target endpoints and assess schema conversion needs.             | 10/29/2025 | 10/29/2025      | <https://000043.awsstudygroup.com/>                                                                 |
| 6   | Migrate & validate: run a small migration demo (e.g., import VM or copy data), validate target resource behavior, and collect logs/metrics. | 10/30/2025 | 10/30/2025      | <https://cloudjourney.awsstudygroup.com/2-migrate/>, <https://000011.awsstudygroup.com/>            |
| 7   | Create migration checklist & cleanup: document steps, rollback procedure, and remove test artifacts (S3, temp roles, DMS tasks).            | 10/31/2025 | 10/31/2025      | <https://cloudjourney.awsstudygroup.com/2-migrate/>, <https://000011.awsstudygroup.com/11-cleanup/> |

### Week 11 Achievements

- Configured CLI profiles and automated inventory collection of candidate workloads.
- Completed a small VM import/export or staged artifact transfer to S3 for migration testing.
- Reviewed database migration patterns (DMS/SCT) and documented conversion considerations.
- Validated a migrated resource and produced a migration checklist with rollback/cleanup steps.

### Quick Reference (CLI examples)

Note: replace placeholders with real values from your environment.

```bash
# Verify CLI profile identity
aws sts get-caller-identity --profile migration

# List EC2 instances and output JSON
aws ec2 describe-instances --profile migration --output json > inventory-instances.json

# Sync VM artifacts to S3 (stage for import)
aws s3 sync ./vm-artifacts s3://fcj-migration-bucket/vm-artifacts --profile migration

# Start VM Import task (example assumes IAM role and S3 staging prepared)
aws ec2 import-image --description "FCJ VM import" --disk-containers file://containers.json --profile migration

# Create a DMS replication instance (example)
aws dms create-replication-instance --replication-instance-identifier fcj-dms --allocated-storage 50 --replication-instance-class dms.t3.medium --profile migration

# Start a DMS task (assumes endpoints and replication instance exist)
aws dms start-replication-task --replication-task-arn arn:aws:dms:... --start-replication-task-type start-replication --profile migration

# Cleanup example: delete staging S3 objects
aws s3 rm s3://fcj-migration-bucket/vm-artifacts --recursive --profile migration
```

### References

- Getting Started with the AWS CLI: <https://000011.awsstudygroup.com/>
- Migrate to AWS overview: <https://cloudjourney.awsstudygroup.com/2-migrate/>
- VM import/export workshop: <https://000014.awsstudygroup.com/>
- Database Migration (DMS) workshop: <https://000043.awsstudygroup.com/>

---

If you'd like, I can:
- Expand each day with full step-by-step CLI commands and safe verification checks.
- Produce CloudFormation/Terraform snippets to automate parts of the migration pipeline (staging S3, IAM roles, DMS instance).
- Create a Vietnamese translation of this page.


