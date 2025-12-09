---
title: "Week 4 Worklog"
date: "2025-08-23"
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---



### Week 4 Objectives (IAM Roles on EC2)

This week focuses on authorizing applications to access AWS services securely by using IAM roles for EC2 rather than long-lived access keys.

* Understand IAM basics and why access keys for application access are discouraged.
* Configure IAM roles with proper policies and trust relationships for EC2 instances.
* Demonstrate how applications use instance profile credentials to access AWS APIs without embedding secrets.
* Practice creating and deleting keys, testing access via AWS CLI/SDK, and cleaning resources.

### Tasks to be carried out this week (aligned with https://000048.awsstudygroup.com/)
| Day | Task                                                                                                                                                                                       | Start Date | Completion Date | Reference Material                               |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ------------------------------------------------ |
| 1   | Module 1 — Prepare: Review IAM basics, instance profiles, and prerequisites. Ensure you have AWS CLI configured and an EC2 Key Pair if needed.                                             | 08/23/2025 | 08/23/2025      | <https://000048.awsstudygroup.com/1-prepare/>    |
| 2   | Module 2 — Use access key: Generate a temporary Access Key/Secret and test how apps use keys to access services. Learn the drawbacks and risks of long-lived credentials.                  | 08/24/2025 | 08/24/2025      | <https://000048.awsstudygroup.com/2-accesskey/>  |
| 3   | Module 2 — Secure key management: Rotate and revoke keys; verify access control; confirm apps fail when keys are removed. Understand why this is not ideal for production.                 | 08/25/2025 | 08/25/2025      | <https://000048.awsstudygroup.com/2-accesskey/>  |
| 4   | Module 3 — IAM Role for EC2: Create IAM role and policy granting least privilege (e.g., S3 read). Set trust relationship and attach to EC2.                                                | 08/26/2025 | 08/26/2025      | <https://000048.awsstudygroup.com/3-iamroleec2/> |
| 5   | Module 3 — Instance Profile Validation: Launch EC2 with the IAM role, verify the application or CLI can access services without Access Keys, and confirm credentials rotate automatically. | 08/27/2025 | 08/27/2025      | <https://000048.awsstudygroup.com/3-iamroleec2/> |
| 6   | Module 3 — Application changes: Update app configuration to use role-based API access; test S3 or DynamoDB access from app without local credentials.                                      | 08/28/2025 | 08/28/2025      | <https://000048.awsstudygroup.com/3-iamroleec2/> |
| 7   | Module 4 — Cleanup and Documentation: Remove test Access Keys, detach IAM role if needed, terminate instances, and document steps and security lessons learned.                            | 08/29/2025 | 08/29/2025      | <https://000048.awsstudygroup.com/4-cleanup/>    |


### Week 4 Achievements (aligned with https://000048.awsstudygroup.com/):

* Completed the IAM-based workflow for EC2: prepared prerequisites and validated the difference between Access Keys and IAM Roles for instances.
* Generated temporary Access Keys for testing, recorded the risk and steps to rotate/revoke them, and confirmed applications can be locked down.
* Created an IAM role with least-privilege policy, attached it to an instance profile, and verified role-based API access from the EC2 instance.
* Updated application configuration to use instance profile credentials and validated access to AWS services (e.g., S3) without embedding credentials.
* Completed resource cleanup by removing test keys, detaching roles if needed, terminating instances, and capturing a short summary of lessons learned.

---

### Quick Reference / Commands

```bash
# Create a user (if needed for test cases) and create access key (DO NOT use in production):
aws iam create-user --user-name test-user
aws iam create-access-key --user-name test-user

# Create an IAM role with trust policy for EC2
aws iam create-role --role-name EC2RoleForApp --assume-role-policy-document file://ec2-trust-policy.json

# Attach policy to role (example: S3 read access)
aws iam attach-role-policy --role-name EC2RoleForApp --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess

# Attach role to instance profile and verify
aws iam create-instance-profile --instance-profile-name EC2RoleForAppProfile
aws iam add-role-to-instance-profile --instance-profile-name EC2RoleForAppProfile --role-name EC2RoleForApp

# Run instance with the instance profile (example CLI)
aws ec2 run-instances --image-id ami-xxxxxxxx --count 1 --instance-type t3.micro --iam-instance-profile Name=EC2RoleForAppProfile --key-name my-key --security-group-ids sg-xxxx --subnet-id subnet-xxxx

# Revoke/Delete access key
aws iam delete-access-key --user-name test-user --access-key-id AKIAxxxxxxxxx

``` 

### References

- IAM Role on EC2: <https://000048.awsstudygroup.com/3-iamroleec2/>
- Use Access Key (risk & rotation): <https://000048.awsstudygroup.com/2-accesskey/>
- Preparation & Cleanup: <https://000048.awsstudygroup.com/1-prepare/> and <https://000048.awsstudygroup.com/4-cleanup/>

