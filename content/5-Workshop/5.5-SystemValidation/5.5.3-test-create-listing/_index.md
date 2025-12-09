---
title: "Test Create Listing"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 5.5.3. </b> "
---

#### Test Case 2: Create a Listing (Protected Route)

Now, let's test if the permissions are working. Only authenticated users (Landlords/Admins) can create a boarding house listing.

This tests Lambda's connection to DynamoDB.

#### Request Details

**Method:** `POST`

**URL:** `<ApiUrl>/landlord/listings`

**Headers:**

- `Authorization: Bearer <YOUR_ACCESS_TOKEN>`
- `Content-Type: application/json`

**Body (JSON):**

```json
{
  "title": "Luxury Apartment in District 1",
  "description": "Full furniture, near manufacturing hub",
  "price": 5000000,
  "address": "123 Le Loi, District 1, HCMC",
  "area": 45,
  "amenities": ["Wifi", "AC", "Parking"]
}
```

{{% notice info %}}
Replace `<YOUR_ACCESS_TOKEN>` with the token you copied from the previous test (Admin Login).
{{% /notice %}}

#### Execute with cURL

```bash
curl -X POST <YOUR_API_URL>/landlord/listings \
     -H "Authorization: Bearer <PASTE_TOKEN_HERE>" \
     -H "Content-Type: application/json" \
     -d '{
           "title": "Luxury Apartment in District 1",
           "description": "Full furniture",
           "price": 5000000,
           "address": "123 Le Loi",
           "area": 45,
           "amenities": ["Wifi", "AC"]
         }'
```

**Example:**

```bash
curl -X POST https://xyz123.execute-api.us-east-1.amazonaws.com/prod/landlord/listings \
     -H "Authorization: Bearer eyJraWQiOiJxxx..." \
     -H "Content-Type: application/json" \
     -d '{
           "title": "Luxury Apartment in District 1",
           "description": "Full furniture, near manufacturing hub",
           "price": 5000000,
           "address": "123 Le Loi, District 1, HCMC",
           "area": 45,
           "amenities": ["Wifi", "AC", "Parking"]
         }'
```

#### Expected Response (201 Created)

```json
{
  "message": "Listing created successfully",
  "data": {
    "listingId": "listing_12345...",
    "title": "Luxury Apartment in District 1",
    "price": 5000000,
    "address": "123 Le Loi, District 1, HCMC",
    "area": 45,
    "amenities": ["Wifi", "AC", "Parking"],
    "createdAt": "2023-10-27T10:30:00Z"
  }
}
```

{{% notice success %}}
If you receive a `201 Created` response with the listing data, your protected route and DynamoDB integration are working correctly!
{{% /notice %}}

#### What This Test Validates

- ✅ **JWT Token Verification**: Lambda validates the access token
- ✅ **Authorization**: Only authenticated users can create listings
- ✅ **DynamoDB Write**: Lambda successfully writes data to DynamoDB
- ✅ **Data Persistence**: The listing is stored in the database

#### Troubleshooting

Common errors:

- **401 Unauthorized**: Token is invalid or expired. Login again to get a fresh token
- **403 Forbidden**: User doesn't have the required permissions
- **400 Bad Request**: Check the JSON body format and required fields
- **500 Internal Server Error**: Check Lambda logs for DynamoDB permission issues

#### Using Postman

If using Postman:

1. Create a new POST request
2. Set URL to `<YOUR_API_URL>/landlord/listings`
3. Go to **Headers** tab:
   - Add `Authorization: Bearer <YOUR_TOKEN>`
   - Add `Content-Type: application/json`
4. Go to **Body** tab → Select **raw** → Choose **JSON**
5. Paste the JSON body
6. Click **Send**
