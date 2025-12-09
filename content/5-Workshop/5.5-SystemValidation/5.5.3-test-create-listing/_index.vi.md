---
title: "Test tạo Listing"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 5.5.3. </b> "
---

#### Test Case 2: Tạo Listing (Protected Route)

Bây giờ, hãy kiểm tra xem quyền hạn có hoạt động không. Chỉ người dùng đã xác thực (Landlords/Admins) mới có thể tạo listing nhà trọ.

Test này kiểm tra kết nối của Lambda đến DynamoDB.

#### Chi tiết Request

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
Thay thế `<YOUR_ACCESS_TOKEN>` bằng token bạn đã sao chép từ test trước (Admin Login).
{{% /notice %}}

#### Thực thi với cURL

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

**Ví dụ:**

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

#### Response mong đợi (201 Created)

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
Nếu bạn nhận được response `201 Created` với dữ liệu listing, protected route và tích hợp DynamoDB của bạn đang hoạt động chính xác!
{{% /notice %}}

#### Test này xác thực điều gì

- ✅ **Xác minh JWT Token**: Lambda xác thực access token
- ✅ **Phân quyền**: Chỉ người dùng đã xác thực mới có thể tạo listings
- ✅ **Ghi DynamoDB**: Lambda ghi dữ liệu thành công vào DynamoDB
- ✅ **Lưu trữ dữ liệu**: Listing được lưu trong database

#### Khắc phục sự cố

Các lỗi thường gặp:

- **401 Unauthorized**: Token không hợp lệ hoặc hết hạn. Đăng nhập lại để lấy token mới
- **403 Forbidden**: User không có quyền cần thiết
- **400 Bad Request**: Kiểm tra format JSON body và các trường bắt buộc
- **500 Internal Server Error**: Kiểm tra Lambda logs để tìm vấn đề về quyền DynamoDB

#### Sử dụng Postman

Nếu dùng Postman:

1. Tạo POST request mới
2. Đặt URL thành `<YOUR_API_URL>/landlord/listings`
3. Vào tab **Headers**:
   - Thêm `Authorization: Bearer <YOUR_TOKEN>`
   - Thêm `Content-Type: application/json`
4. Vào tab **Body** → Chọn **raw** → Chọn **JSON**
5. Dán JSON body
6. Click **Send**
