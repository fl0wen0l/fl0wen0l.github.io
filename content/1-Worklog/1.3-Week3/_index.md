---
title: "Week 3 Worklog"
date: "2025-08-16"
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---
 
### Week 3 Objectives:

* Introduce Amazon Elastic Compute Cloud (EC2), including instance types, AMIs, and storage options.
* Prepare and provision network infrastructure (VPCs) and firewall policies for Linux and Windows workloads.
* Launch and configure Microsoft Windows Server 2022 and Amazon Linux instances.
* Manage EC2 instance lifecycle (modify configuration, snapshots, AMIs, and recovery options).
* Deploy sample applications (User Management App and Node.js app) for real-world validation.
* Apply IAM best practices for cost & usage governance, and document cleanup procedures.

### Introduction: What is Amazon EC2?

Amazon Elastic Compute Cloud (EC2) is a web service that provides secure, resizable compute capacity in the cloud. EC2 instances are virtual servers you can use to run applications. Key concepts covered this week:

- Amazon EC2 Instance Types: General purpose, compute-optimized, memory-optimized, etc.
- Amazon Machine Images (AMI): Pre-configured OS and application templates used to launch instances.
- Storage Options: EBS (Elastic Block Store), Instance store, EBS-backed AMIs.

Reference: <https://000004.awsstudygroup.com/>

---

### Preparation (follow Module 2: Prerequisite)

Follow the EC2 workshop prerequisite module for all preparation steps. Key items include:

- Ensure you have an AWS account and a default VPC or prepare a VPC/subnet for the lab as instructed in Module 2.
- Create Security Groups required for Linux and Windows instances (SSH, RDP, HTTP, HTTPS as needed).
- Create or import Key Pairs for Linux and confirm retrieval method for the Windows Server admin password.
- Create an IAM role for EC2 with SSM permissions and any S3 access required for snapshot or backup operations.

Reference: <https://000004.awsstudygroup.com/2-prerequiste>

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                                                     | Start Date | Completion Date | Reference Material                               |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------- | --------------- | ------------------------------------------------ |
| 1   | Introduction & Preparation: Learn EC2 types/AMIs/storage; create VPCs and subnets; create Key Pairs and IAM role for EC2; design security group rules for Linux and Windows instances.                                   | 08/16/2025 | 08/16/2025      | <https://000004.awsstudygroup.com/>              |
| 2   | Preparation & Network: Follow the EC2 prerequisite module to prepare network settings, security groups for Linux (SSH/HTTP/HTTPS) and Windows (RDP/HTTP/HTTPS), and key pair/credentials.                                | 08/17/2025 | 08/17/2025      | <https://000004.awsstudygroup.com/2-prerequiste> |
| 3   | Harden security and verify connectivity: Configure Security Groups, attach Key Pairs, and validate access (RDP/SSH) to prepared instances.                                                                               | 08/18/2025 | 08/18/2025      | <https://000004.awsstudygroup.com/2-prerequiste> |
| 4   | Launch Microsoft Windows Server 2022 instance: Launch Windows Server 2022 AMI, configure admin password, add to Security Group, assign Elastic IP and connect via RDP.                                                   | 08/19/2025 | 08/19/2025      | <https://000004.awsstudygroup.com/>              |
| 5   | Launch Amazon Linux instance: Launch Amazon Linux 2 AMI, attach Key Pair, configure Security Group for SSH; connect via SSH and attach an EBS volume for data.                                                           | 08/20/2025 | 08/20/2025      | <https://000004.awsstudygroup.com/>              |
| 6   | EC2 Basics & Hardening: Modify instance configuration (instance type, user-data), create and manage EBS snapshots, build custom AMIs (optional), test recovery via Systems Manager and user-data.                        | 08/21/2025 | 08/21/2025      | <https://000004.awsstudygroup.com/>              |
| 7   | Deploy Applications & Governance: Deploy AWS User Management App on Amazon Linux 2; deploy Node.js app on EC2 Windows; implement cost and usage governance with IAM roles and resource tags; cleanup and document steps. | 08/22/2025 | 08/22/2025      | <https://000004.awsstudygroup.com/9-cleanup/>    |

---

### Launch Microsoft Windows Server 2022 Instance

1. Create or select a Windows Server 2022 AMI (Region-specific).
2. Choose instance type as required (e.g., t3.medium for testing).
3. Select the Windows security group that allows RDP (3389), HTTP (80) and HTTPS (443).
4. Attach an EBS volume and (optionally) assign an Elastic IP for remote access.
5. Retrieve the administrator password via EC2 console and connect via RDP.

### Launch Amazon Linux Instance

1. Launch Amazon Linux 2 AMI with an appropriate instance type (e.g., t3.micro for testing).
2. Attach the Linux security group enabling SSH (22), HTTP (80) and HTTPS (443).
3. Connect using the Key Pair and SSH, and verify network connectivity.
4. Attach and mount an additional EBS volume for data storage.

### Amazon EC2 Basic Operations

* Modify EC2 Instance Configuration: Change instance types, add or remove EBS volumes, and edit instance tags.
* Create and Manage EBS Snapshots: Snapshot volumes before major changes, store snapshots in separate S3 buckets or AMIs.
* Build Custom AMIs: After configuring an instance, create an AMI for repeatable deployments.
* Launch Instances from Custom AMIs: Validate the AMI by spinning up a new instance from it.
* Recover Access to Windows Instances Using AWS Systems Manager (SSM): Use SSM Session Manager or EC2Rescue (if SSM is enabled).
* Recover Access to Linux Instances Using User Data: Use cloud-init user-data scripts or attach a recovery instance to mount the root volume and fix SSH configuration.
* Configure GUI Desktop Environment for Ubuntu (Optional): Install and configure a lightweight desktop (e.g., XFCE) for testing or administrative convenience.
* Implement EBS Volume Archiving: Use snapshot lifecycle, move older snapshots to lower-cost storage, or export to S3 for long-term retention.

### Deploying Applications

* Deploying an AWS User Management Application on Amazon Linux 2:
  - Prepare the instance, install dependencies (e.g., Node.js, Nginx, database client), and clone the user management app from a repo.
  - Configure environment variables, start the service (systemd), and confirm connectivity using the public IP or a load balancer.

* Deploying Node.js Applications on Amazon EC2 Windows:
  - Install Node.js and required runtime tools on Windows Server 2022.
  - Deploy the application, configure IIS or a reverse-proxy like Nginx for Windows, and test the endpoint.

### Cost & Usage Governance with IAM

* Create IAM users with least-privilege policies for day-to-day operations.
* Use IAM roles for EC2 instances to avoid embedding credentials.
* Use resource tags to segregate test and dev resources for cost allocation.
* Monitor costs using AWS Cost Explorer and create budgets & alerts for accounts.

### Week 3 Achievements:

* Gained a practical understanding of Amazon EC2, instance types, AMIs and storage options.
* Created VPCs and applied carefully designed Security Groups to protect both Linux and Windows workloads.
* Launched and connected to Microsoft Windows Server 2022 and Amazon Linux 2 instances; attached EBS volumes and verified I/O.
* Performed EBS snapshot operations and created a custom AMI for repeatable deployments.
* Validated instance recovery methods using AWS Systems Manager and user-data-based recovery workflows.
* Deployed a sample User Management app on Amazon Linux 2 and a Node.js app on Windows for functional testing.
* Implemented IAM best practices and resource tagging to support cost & usage governance.
* Documented all steps with commands, templates, and learnings for reproducibility.

---

### Quick Reference / Commands

```bash
aws ec2 describe-instances --output table
aws ec2 create-snapshot --volume-id vol-xxxxxxxx --description "pre-change snapshot"
aws ec2 create-image --instance-id i-xxxxxxxx --name "Custom-AMI-Week3" --no-reboot
aws ec2 run-instances --image-id ami-xxxxxxxx --count 1 --instance-type t3.micro --key-name my-key --security-group-ids sg-xxxxxxxx --subnet-id subnet-xxxxxxxx
aws ec2 create-tags --resources i-xxxxxxxx --tags Key=Project,Value=Week3-EC2
```

### References:

- Amazon EC2 overview: <https://000004.awsstudygroup.com/>
- EC2 workshop home: <https://000004.awsstudygroup.com/>

