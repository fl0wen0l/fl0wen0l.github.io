---
title: "Event 3"
date: "2025-11-29"
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---


**AWS Cloud Mastery Series #3 — Well‑Architected Security Pillar**

Date: **Saturday, 29 November 2025 | 08:30 - 12:00 (GMT+7)**

Location: **Bitexco Financial Tower, District 1, Ho Chi Minh City**

## Personal Reflection — Security Pillar Workshop

Attending the **AWS Well‑Architected: Security Pillar** session gave me a structured and practical view of cloud security. The workshop covered five main areas — IAM, Detection, Infrastructure Protection, Data Protection, and Incident Response — and tied them together with real examples and recommended practices.

Key learnings I took away:

- Identity & Access Management: Reinforced the importance of avoiding long‑term credentials, using roles and short‑lived credentials, adopting SSO through IAM Identity Center, and applying SCPs/permission boundaries for multi‑account setups.
- Detection & Logging: Emphasized comprehensive logging (CloudTrail at org level, VPC Flow Logs, ALB/S3 logs), enabling GuardDuty and Security Hub, and using EventBridge for automated alerting. The idea of Detection‑as‑Code stood out as a practical way to version and test detection rules.
- Network & Workload Security: Covered VPC segmentation, private vs public placement, pragmatic use of Security Groups vs NACLs, and protections like WAF, Shield, and Network Firewall to reduce the attack surface.
- Data Protection: Clear patterns for encryption at rest and in transit using KMS, key policies and rotation, plus using Secrets Manager/Parameter Store for secrets management and automated rotation.
- Incident Response (IR): Walked through IR playbooks for common scenarios (compromised IAM key, accidental S3 public exposure, EC2 malware). I appreciated the actionable steps: isolate, snapshot, collect evidence, and automate response using Lambda/Step Functions.

What impressed me most:

- The practicality of IR playbooks — they were not theoretical but included clear steps and tools to execute during an incident.
- Detection‑as‑Code: treating detection logic like application code enables reviews, testing, and CI/CD for security rules.
- Multi‑account IAM design: examples of permission boundaries and SCPs made the least‑privilege principle tangible at scale.

How I'll apply this right away:

1. Audit IAM in my projects: remove long‑term credentials, enforce MFA, and review permission scopes for roles.
2. Ensure CloudTrail is enabled at the organization level and centralize logs for analysis and retention.
3. Draft two IR playbooks (compromised key and S3 public exposure) with explicit steps and scripts for evidence collection.
4. Prototype a Detection‑as‑Code workflow: store detection rules in a repo and deploy them via CI to a test account.

Conclusion:

This workshop connected high‑level security principles to actionable practices I can start implementing immediately. It gave me a clear to‑do list to improve the security posture of small projects within the next few weeks.



