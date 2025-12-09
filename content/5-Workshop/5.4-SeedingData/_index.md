---
title: "Seeding Data"
date: "`r Sys.Date()`"
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

#### Why seed data?

Your infrastructure is ready, but the database is empty. You need a **"Super Admin"** account to manage the system.

We will run a Node.js script that connects directly to AWS to:

- Create a User in Cognito User Pool
- Add that user to the Admins Group
- Create a corresponding profile in the DynamoDB UserProfiles table

#### Content

- [Setup the Script](5.4.1-setup-script/)
- [Configure Environment](5.4.2-configure-environment/)
- [Run the Script](5.4.3-run-script/)
- [Verification](5.4.4-verification/)
