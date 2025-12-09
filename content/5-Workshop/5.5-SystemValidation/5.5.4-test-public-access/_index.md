---
title: "Test Public Access"
date: "`r Sys.Date()`"
weight: 4
chapter: false
pre: " <b> 5.5.4. </b> "
---

#### Test Case 3: Public Access (Listings)

Finally, verify that public users can view listings without logging in.

This validates that your API has both protected and public routes working correctly.

#### Request Details

**Method:** `GET`

**URL:** `<ApiUrl>/listings`

**Headers:** None required (public endpoint)

#### Execute with cURL

```bash
curl <YOUR_API_URL>/listings
```

**Example:**

```bash
curl https://xyz123.execute-api.us-east-1.amazonaws.com/prod/listings
```

#### Expected Response (200 OK)

You should see an array containing the listing you just created in Test Case 2:

```json
{
  "data": [
    {
      "listingId": "listing_12345...",
      "title": "Luxury Apartment in District 1",
      "description": "Full furniture, near manufacturing hub",
      "price": 5000000,
      "address": "123 Le Loi, District 1, HCMC",
      "area": 45,
      "amenities": ["Wifi", "AC", "Parking"],
      "createdAt": "2023-10-27T10:30:00Z",
      "landlordId": "..."
    }
  ],
  "message": "Listings retrieved successfully"
}
```

{{% notice success %}}
If you can see the listing without authentication, your public API route is working correctly!
{{% /notice %}}

#### What This Test Validates

- ✅ **Public Access**: Unauthenticated users can view listings
- ✅ **DynamoDB Read**: Lambda successfully reads data from DynamoDB
- ✅ **Data Retrieval**: Previously created data is accessible
- ✅ **API Routing**: Both protected and public routes coexist properly

#### Additional Tests

You can also test with query parameters:

**Search by location:**

```bash
curl "<YOUR_API_URL>/listings?location=District%201"
```

**Filter by price range:**

```bash
curl "<YOUR_API_URL>/listings?minPrice=3000000&maxPrice=6000000"
```

**Pagination:**

```bash
curl "<YOUR_API_URL>/listings?page=1&limit=10"
```

#### Using a Browser

Since this is a GET request, you can simply open the URL in your web browser:

```
https://xyz123.execute-api.us-east-1.amazonaws.com/prod/listings
```

The browser will display the JSON response directly.

#### Troubleshooting

- **404 Not Found**: Verify the endpoint path is correct (`/listings` not `/listing`)
- **Empty Array**: No listings in database yet, create one first (Test Case 2)
- **500 Internal Server Error**: Check Lambda logs for DynamoDB read permission issues
- **CORS Error (in browser)**: Check API Gateway CORS configuration

{{% notice tip %}}
Use a browser extension like **JSON Formatter** for better readability of JSON responses in the browser.
{{% /notice %}}
