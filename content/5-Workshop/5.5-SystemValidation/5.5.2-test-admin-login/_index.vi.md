---
title: "Test đăng nhập Admin"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 5.5.2. </b> "
---

#### Test Case 1: Đăng nhập Admin

Đây là test quan trọng nhất. Nó xác minh:

- ✅ API Gateway định tuyến traffic đúng đến Lambda
- ✅ Lambda có thể kết nối đến Cognito
- ✅ Cognito xác thực thông tin đăng nhập đã tạo ở Phần 5.4

#### Chi tiết Request

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
Thay thế email và password bằng thông tin đăng nhập bạn đã tạo ở Phần 5.4 (Seeding Data).
{{% /notice %}}

#### Thực thi với cURL

```bash
curl -X POST <YOUR_API_URL>/auth/login \
     -H "Content-Type: application/json" \
     -d '{"email": "admin@findnest.com", "password": "Password@123"}'
```

**Ví dụ:**

```bash
curl -X POST https://xyz123.execute-api.us-east-1.amazonaws.com/prod/auth/login \
     -H "Content-Type: application/json" \
     -d '{"email": "admin@findnest.com", "password": "Password@123"}'
```

#### Response mong đợi (200 OK)

Bạn sẽ nhận được JSON object chứa authentication tokens (ID Token, Access Token):

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

#### Hành động quan trọng

**Sao chép `accessToken` từ response.** Chúng ta sẽ cần nó cho test case tiếp theo.

{{% notice success %}}
Nếu bạn nhận được tokens, chúc mừng! Hệ thống xác thực của bạn đang hoạt động chính xác.
{{% /notice %}}

#### Khắc phục sự cố

Nếu gặp lỗi:

- **400 Bad Request**: Kiểm tra JSON body được format đúng
- **401 Unauthorized**: Xác minh email và password khớp với những gì bạn tạo ở Phần 5.4
- **500 Internal Server Error**: Kiểm tra Lambda logs trong CloudWatch Logs
- **Network Error**: Xác minh API URL đúng và có thể truy cập

#### Sử dụng Postman

Nếu bạn thích dùng Postman:

1. Tạo POST request mới
2. Đặt URL thành `<YOUR_API_URL>/auth/login`
3. Vào tab **Body** → Chọn **raw** → Chọn **JSON**
4. Dán JSON body
5. Click **Send**
6. Sao chép `accessToken` từ response
