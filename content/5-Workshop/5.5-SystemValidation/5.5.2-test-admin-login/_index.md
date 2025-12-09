---
title: "Test Admin Login"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 5.5.2. </b> "
---

#### Test Case 1: Admin Login

This is the most critical test. It verifies:

- ✅ API Gateway correctly routes traffic to Lambda
- ✅ Lambda can connect to Cognito
- ✅ Cognito validates the credentials seeded in Section 5.4

#### Request Details

**Method:** `POST`

**URL:** `<ApiUrl>/auth/login`

**Body (JSON):**

```json
{
  "email": "admin@findnest.com",
  "password": "Password@123"
}
```

{{% notice warning %}}
Replace the email and password with the credentials you created in Section 5.4 (Seeding Data).
{{% /notice %}}

#### Execute with cURL

```bash
curl -X POST <YOUR_API_URL>/auth/login \
     -H "Content-Type: application/json" \
     -d '{"email": "admin@findnest.com", "password": "Password@123"}'
```

**Example:**

```bash
curl -X POST https://xyz123.execute-api.us-east-1.amazonaws.com/prod/auth/login \
     -H "Content-Type: application/json" \
     -d '{"email": "admin@findnest.com", "password": "Password@123"}'
```

#### Expected Response (200 OK)

You should receive a JSON object containing the authentication tokens (ID Token, Access Token):

```json
{
  "data": {
    "id": "...",
    "email": "admin@findnest.com",
    "accessToken": "eyJraWQiOiJ...",
    "refreshToken": "eyJjdHkiOiJ...",
    "idToken": "eyJraWQiOiJ...",
    "role": "Admins"
  },
  "message": "Login successful"
}
```

#### Important Action

**Copy the `accessToken` from the response.** We will need it for the next test case.

{{% notice success %}}
If you receive the tokens, congratulations! Your authentication system is working correctly.
{{% /notice %}}

#### Troubleshooting

If you encounter errors:

- **400 Bad Request**: Check that the JSON body is formatted correctly
- **401 Unauthorized**: Verify the email and password match what you created in Section 5.4
- **500 Internal Server Error**: Check Lambda logs in CloudWatch Logs
- **Network Error**: Verify the API URL is correct and accessible

#### Using Postman

If you prefer using Postman:

1. Create a new POST request
2. Set URL to `<YOUR_API_URL>/auth/login`
3. Go to **Body** tab → Select **raw** → Choose **JSON**
4. Paste the JSON body
5. Click **Send**
6. Copy the `accessToken` from the response
