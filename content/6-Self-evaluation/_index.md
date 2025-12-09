---
title: "Self-Assessment"
date: "2025-11-25"
weight: 6
chapter: false
pre: " <b> 6. </b> "
---


During my internship at **[Amazone Web Service - AWS]** from **[06-09-2025]** to **[09-12-2025]**, I had the opportunity to learn, practice, and apply the knowledge acquired in school to a real-world working environment.  
I participated in **[briefly describe the main project or task]**, through which I improved my skills in **[list skills: programming, analysis, reporting, communication, etc.]**.  

In terms of work ethic, I always strived to complete tasks well, complied with workplace regulations, and actively engaged with colleagues to improve work efficiency.

To reflect on my internship in a constructive and actionable way, I reorganised my self-evaluation into clear sections: objectives, key accomplishments, skills developed, concrete examples, challenges faced and how I addressed them, feedback received, and development goals going forward.

**Internship context**
- Role: [Backend + Security]
- Team/Project: [Find Nest]
- Period: **[15-09-2025]** – **[07-12-2025]**

**Objectives I set at the start**
- Learn and apply cloud fundamentals (AWS networking, S3, VPC endpoints).
- Implement and document a secure hybrid access lab for S3 using VPC endpoints.
- Improve scripting and automation skills (CloudFormation, bash, AWS CLI).
- Enhance teamwork, reporting, and presentation abilities.

**Key achievements**
- Designed and authored a full end‑to‑end workshop: "Secure Hybrid Access to S3 using VPC Endpoints" including architecture diagrams, step‑by‑step labs, and cleanup procedures.
- Implemented the CloudFormation stacks used in the workshop and validated deployment in the `us-east-1` region (reduced manual setup time for participants by ~15 minutes).
- Created reproducible test procedures for Gateway and Interface endpoints, and produced troubleshooting notes for common DNS and VPN issues.
- Wrote clear documentation and translated core workshop materials into Vietnamese and English to support a broader audience.

**Skills developed**
- Technical: AWS VPC, Route 53 Resolver, Site‑to‑Site VPN simulation (strongSwan), S3 endpoints (gateway & interface), CloudFormation, AWS CLI, Session Manager.
- Software: Shell scripting (fallocate, aws cli automation), Markdown documentation and diagrams.
- Soft skills: Technical writing, knowledge transfer (workshop delivery), teamwork and cross‑functional collaboration.

**Concrete examples / Evidence**
- Workshop content: `content/5-Workshop/*` (architecture, lab steps, cleanup).
- Automation: CloudFormation template links included in the lab (PLCloudSetup, PLOnpremSetup).
- Tests performed: Upload/download operations via Session Manager on EC2 instances using both Gateway and Interface endpoints.

**Challenges and mitigation**
- DNS resolution for PrivateLink endpoints was initially inconsistent across simulated on‑prem DNS; I implemented a Route 53 resolver forwarding rule and documented the exact steps to reproduce and fix the issue.
- Large file uploads (1GB test files) occasionally hit timeouts; I added guidance to use smaller sample files when bandwidth is constrained and included tips to monitor and retry uploads.

**Feedback received & actions taken**
- Feedback: Request to make labs more beginner friendly and to provide shorter quick‑start exercises.
- Action: Added a quick start "smoke test" that performs minimal checks and a separate deep‑dive section for advanced troubleshooting.

**Development areas (plan)**
1. Discipline & time management: adopt a weekly task plan with prioritized tickets and short daily notes; review progress with mentor.
2. Problem solving: practice structured troubleshooting (hypothesis→test→isolate) and log findings to a shared knowledge base.
3. Communication: present one lab walkthrough to the team and request focused feedback on clarity and pacing.

**Goals for the next 6 months**
- Contribute to at least two more workshop modules and automate one end‑to‑end test with CI (e.g., a basic CloudFormation validation run).
- Complete an AWS certification or internal training module related to networking or security.

**Final reflection**
This internship provided a strong bridge between theoretical study and practical engineering. I improved both technical capabilities (networking, AWS services, automation) and professional skills (documentation, collaboration, feedback handling). I will continue to iterate on the materials I created and use the feedback I received to make the next workshops clearer and more resilient for participants.

### Quick self-assessment summary (snapshot)
| Area                         | Rating (1–5) | Notes                                                               |
| ---------------------------- | -----------: | ------------------------------------------------------------------- |
| Technical knowledge          |            4 | Solid understanding of VPC endpoints and Route 53 resolver patterns |
| Ability to learn             |            4 | Rapidly picked up new AWS services and troubleshooting methods      |
| Proactiveness                |            5 | Took initiative to create workshop materials and automate steps     |
| Responsibility & reliability |            4 | Delivered deliverables on time, with incremental improvements       |
| Communication                |            3 | Improved but needs practice in concise presentations                |
| Problem solving              |            3 | Good at identifying causes; will improve speed and coverage         |


