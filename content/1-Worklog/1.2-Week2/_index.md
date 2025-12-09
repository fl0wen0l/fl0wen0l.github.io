---
title: "Week 2 Worklog"
date: "2025-11-25"
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---
## Overview

This week focuses on designing, building and securing a Virtual Private Cloud (VPC) environment on AWS. The work includes network design and firewall controls, deploying and securing EC2 instances, establishing a site-to-site VPN, and automating the setup using Infrastructure-as-Code templates. Clean-up and documentation are part of the workflow to ensure reproducibility and security.

### Prerequisites

- An AWS account with permissions to create VPCs, EC2, VPN and related resources
- AWS CLI installed and configured (Access Key, Secret Key, Default Region)
- Basic familiarity with CloudFormation or Terraform (for IaC sections)
- Access to the First Cloud Journey reference materials: <https://cloudjourney.awsstudygroup.com/>

> Note: Always follow your organization's security policy and avoid sharing any credentials or sensitive keys. Use ephemeral resources and clean up test resources when done.


### Week 2 Objectives:

* Understand VPC fundamentals and firewall controls (Security Groups & NACLs).
* Design and build a secure VPC with public/private subnets, routing and gateways.
* Deploy and secure EC2 instances, including SSH access and EBS usage.
* Configure a Site-to-Site VPN for hybrid connectivity and test routing.
* Practice Infrastructure-as-Code (CloudFormation/Terraform) to automate deployments.
* Clean up resources and document all steps and learning points.

### Tasks to be carried out this week:
| Day | Task                                                                                                                                                                                            | Start Date | Completion Date | Reference Material                        |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | --------------- | ----------------------------------------- |
| 1   | Study VPC architecture and firewall concepts (Security Groups & NACLs); draw a secure VPC design plan (public/private subnets).                                                                 | 08/11/2025 | 08/11/2025      | <https://000003.awsstudygroup.com/>       |
| 2   | Build the VPC: create VPC, subnets (public/private), route tables, Internet Gateway and NAT Gateway; implement basic Security Groups and NACL baselines.                                        | 08/12/2025 | 08/12/2025      | <https://000003.awsstudygroup.com/>       |
| 3   | Deploy EC2 instances: choose AMI & instance type, launch EC2 within the correct subnet, configure key pairs and IAM role, attach an EBS volume.                                                 | 08/13/2025 | 08/13/2025      | <https://000004.awsstudygroup.com/>       |
| 4   | Harden EC2 networking & firewall: create Security Group rules for SSH/HTTP/HTTPS, test connectivity, assign Elastic IP (if required) and verify routing from subnet to IGW/NAT.                 | 08/14/2025 | 08/14/2025      | <https://000004.awsstudygroup.com/>       |
| 5   | Setup a site-to-site VPN: configure Customer Gateway, Virtual Private Gateway and VPN connection; verify route propagation and test end-to-end connectivity.                                    | 08/15/2025 | 08/15/2025      | <https://000003.awsstudygroup.com/>       |
| 6   | Implement Infrastructure-as-Code (CloudFormation or Terraform): write templates to provision the VPC, subnets, EC2, SGs and VPN resources; test deployment from IaC templates.                  | 08/16/2025 | 08/16/2025      | <https://000037.awsstudygroup.com/>       |
| 7   | Clean up all test resources safely (terminate instances, detach & snapshot volumes if needed, remove VPC), and write a short summary of lessons learned plus documentation for reproducibility. | 08/17/2025 | 08/17/2025      | <https://cloudjourney.awsstudygroup.com/> |


### Week 2 Achievements:

* Designed and provisioned a secure VPC (public & private subnets, IGW, NAT) following best practices.
* Implemented Security Groups and NACLs to enforce network-level firewall controls for EC2 and subnet traffic.
* Deployed EC2 instances using appropriate AMIs and instance types, attached EBS storage, and configured SSH access.
* Configured and validated a Site-to-Site VPN connection, verified routing and basic connectivity between networks.
* Created Infrastructure-as-Code templates (CloudFormation/Terraform) to automate VPC, EC2, and VPN provisioning.
* Cleaned up test resources and documented the deployment steps, templates, and lessons learned for reproducibility.

