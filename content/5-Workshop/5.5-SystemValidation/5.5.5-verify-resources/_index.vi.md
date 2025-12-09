---
title: "Xác minh tài nguyên"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: " <b> 5.5.5. </b> "
---

#### Xác minh tài nguyên trong Console

Nếu tất cả requests đều thành công, hãy xác minh data persistence trong AWS Console.

Điều này xác nhận rằng dữ liệu thực sự được lưu trong DynamoDB, không chỉ trả về từ memory của Lambda.

#### Bước 1: Điều hướng đến DynamoDB Console

1. Truy cập [DynamoDB Console](https://console.aws.amazon.com/dynamodb/)
2. Chọn **Tables** từ sidebar bên trái

Bạn sẽ thấy tất cả các bảng được tạo bởi CDK stack:

- BoardingHouseListings
- UserProfiles
- OTPVerifications
- UserFavorites
- SupportRequests
- SearchHistory
- UserPreferences

#### Bước 2: Khám phá bảng Listings

1. Click vào bảng **BoardingHouseListings**
2. Click nút **Explore table items**

Bạn sẽ thấy listing đã tạo ở Test Case 2.

#### Bước 3: Xác minh dữ liệu

Kiểm tra record chứa:

- **listingId**: Định danh duy nhất tự động tạo
- **title**: "Luxury Apartment in District 1"
- **description**: "Full furniture, near manufacturing hub"
- **price**: 5000000
- **address**: "123 Le Loi, District 1, HCMC"
- **area**: 45
- **amenities**: Array chứa ["Wifi", "AC", "Parking"]
- **createdAt**: Timestamp tạo
- **landlordId**: User ID của admin đã tạo

#### Bước 4: Xác minh User Profile

1. Quay lại Tables
2. Click vào bảng **UserProfiles**
3. Click **Explore table items**

Bạn sẽ thấy admin user profile đã tạo ở Phần 5.4:

- **userId**: Cognito user ID
- **email**: Email admin của bạn
- **fullName**: Tên đầy đủ của admin
- **userType**: "admin"
- **createdAt**: Timestamp

#### Danh sách kiểm tra xác minh

Xác nhận các điều sau:

- ✅ **API Gateway** định tuyến requests chính xác
- ✅ **Lambda** xử lý requests và thực thi business logic
- ✅ **Cognito** xác thực users thành công
- ✅ **DynamoDB** lưu trữ và truy xuất dữ liệu chính xác
- ✅ **Protected routes** yêu cầu xác thực
- ✅ **Public routes** có thể truy cập không cần xác thực
- ✅ **IAM permissions** cho phép Lambda truy cập tất cả services cần thiết

#### Xác thực kiến trúc hệ thống

Bạn đã xác thực thành công một kiến trúc serverless hoàn chỉnh:

```
Client (Postman/cURL)
    ↓
API Gateway (REST API)
    ↓
Lambda Function (Node.js)
    ↓
├─→ Cognito (Authentication)
├─→ DynamoDB (Data Storage)
├─→ SNS (Notifications - chưa test)
├─→ Bedrock (AI - chưa test)
└─→ Location Service (Maps - chưa test)
```

{{% notice success %}}
Chúc mừng! Serverless backend của bạn đã hoàn toàn hoạt động và sẵn sàng để tích hợp frontend.
{{% /notice %}}

#### Các bước tiếp theo

- Test thêm các endpoints (đăng ký user, favorites, search, v.v.)
- Theo dõi Lambda execution trong CloudWatch Logs
- Thiết lập CloudWatch alarms cho lỗi
- Tích hợp với ứng dụng frontend
- Triển khai thêm tính năng (AI chat, tích hợp map, notifications)

{{% notice tip %}}
Giữ API URL và thông tin đăng nhập admin an toàn. Cân nhắc sử dụng environment variables hoặc secrets management trong production.
{{% /notice %}}
