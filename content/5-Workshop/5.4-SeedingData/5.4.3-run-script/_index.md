---
title: "Run the Script"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---

Now, execute the script. It is interactive, so it will ask you for details.

#### Execute the script

```bash
node seed-admin.js
```

#### Interactive Walkthrough

The script will prompt you for the following information:

1. **Username**: Enter a username (e.g., `superadmin`)
2. **Password**: Enter a strong password (min 8 chars, uppercase, lowercase, numbers, special chars)
3. **Email**: Enter a valid email address
4. **Full Name**: Enter a display name
5. **Confirm**: Type `yes` to proceed

#### Expected Output

If everything is configured correctly, you should see output similar to this:

```plaintext
✅ Environment variables loaded:
   Region: us-east-1
   User Pool ID: us-east-1_xxxxxx
   User Profiles Table: UserProfiles

📝 Creating admin user in Cognito...
   ✅ User created with ID: xxxx-xxxx-xxxx
🔑 Setting permanent password...
   ✅ Password set
👑 Adding user to Admins group...
   ✅ Added to Admins group
📝 Creating admin profile in DynamoDB...
   ✅ Profile created

═══════════════════════════════════════════════
✅ ADMIN USER CREATED SUCCESSFULLY!
═══════════════════════════════════════════════
```

{{% notice success %}}
**Save the credentials!** You'll need the username and password to log in to the system.
{{% /notice %}}

{{% notice warning %}}
If you encounter errors:

- Check that your `.env` file has the correct values
- Verify your AWS credentials are configured (`aws configure`)
- Ensure you have the necessary IAM permissions
- Make sure the CDK stack was deployed successfully

{{% /notice %}}

#### What the script does

The script performs the following operations:

1. ✅ **Creates a user in Cognito User Pool** with the provided username and password
2. ✅ **Sets the password as permanent** (no need to change on first login)
3. ✅ **Adds the user to the "Admins" group** for elevated permissions
4. ✅ **Creates a profile record in DynamoDB** with user information
