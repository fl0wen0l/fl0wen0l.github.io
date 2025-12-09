---
title: "Test truy cập công khai"
date: "`r Sys.Date()`"
weight: 4
chapter: false
pre: " <b> 5.5.4. </b> "
---

#### Test Case 3: Truy cập công khai (Listings)

Cuối cùng, xác minh rằng người dùng công khai có thể xem listings mà không cần đăng nhập.

Điều này xác thực rằng API của bạn có cả protected và public routes hoạt động chính xác.

#### Chi tiết Request

**Method:** `GET`

**URL:** `<ApiUrl>/listings`

**Headers:** Không cần (public endpoint)

#### Thực thi với cURL

```bash
curl <YOUR_API_URL>/listings
```

**Ví dụ:**

```bash
curl https://xyz123.execute-api.us-east-1.amazonaws.com/prod/listings
```

#### Response mong đợi (200 OK)

Bạn sẽ thấy một array chứa listing bạn vừa tạo ở Test Case 2:

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
Nếu bạn có thể thấy listing mà không cần xác thực, public API route của bạn đang hoạt động chính xác!
{{% /notice %}}

#### Test này xác thực điều gì

- ✅ **Truy cập công khai**: User không xác thực có thể xem listings
- ✅ **Đọc DynamoDB**: Lambda đọc dữ liệu thành công từ DynamoDB
- ✅ **Truy xuất dữ liệu**: Dữ liệu đã tạo trước đó có thể truy cập được
- ✅ **Định tuyến API**: Cả protected và public routes cùng tồn tại đúng cách

#### Các test bổ sung

Bạn cũng có thể test với query parameters:

**Tìm theo vị trí:**

```bash
curl "<YOUR_API_URL>/listings?location=District%201"
```

**Lọc theo khoảng giá:**

```bash
curl "<YOUR_API_URL>/listings?minPrice=3000000&maxPrice=6000000"
```

**Phân trang:**

```bash
curl "<YOUR_API_URL>/listings?page=1&limit=10"
```

#### Sử dụng trình duyệt

Vì đây là GET request, bạn có thể mở URL trực tiếp trong trình duyệt web:

```
https://xyz123.execute-api.us-east-1.amazonaws.com/prod/listings
```

Trình duyệt sẽ hiển thị JSON response trực tiếp.

#### Khắc phục sự cố

- **404 Not Found**: Xác minh đường dẫn endpoint đúng (`/listings` không phải `/listing`)
- **Empty Array**: Chưa có listings trong database, tạo một cái trước (Test Case 2)
- **500 Internal Server Error**: Kiểm tra Lambda logs về vấn đề quyền đọc DynamoDB
- **CORS Error (trong browser)**: Kiểm tra cấu hình CORS của API Gateway

{{% notice tip %}}
Sử dụng extension trình duyệt như **JSON Formatter** để dễ đọc JSON responses hơn trong trình duyệt.
{{% /notice %}}
