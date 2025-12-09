---
title: "Xác minh"
date: "`r Sys.Date()`"
weight: 4
chapter: false
pre: " <b> 5.4.4. </b> "
---

Hãy đảm bảo user tồn tại trong cả dịch vụ Authentication và Database.

#### A. Kiểm tra Cognito (Authentication)

1. Truy cập [Amazon Cognito Console](https://console.aws.amazon.com/cognito/)
2. Chọn **User Pool** `FindNestUsers`
3. Đi tới tab **Users**

Bạn sẽ thấy `superadmin` với trạng thái **Confirmed**.

Click vào user và xác minh họ nằm trong nhóm **Admins**.

{{% notice info %}}
User phải có:

- Trạng thái: **CONFIRMED** (không phải FORCE_CHANGE_PASSWORD)
- Thành viên nhóm: **Admins**

{{% /notice %}}

#### B. Kiểm tra DynamoDB (Data)

1. Truy cập [DynamoDB Console](https://console.aws.amazon.com/dynamodb/)
2. Chọn bảng **UserProfiles**
3. Click **Explore table items**

Tìm kiếm item có `userId` khớp với ID từ output của script.

Bạn sẽ thấy dữ liệu profile với `userType: "admin"`.

#### Danh sách kiểm tra

Xác nhận các điều sau trước khi tiếp tục:

- ✅ User tồn tại trong Cognito User Pool với trạng thái **CONFIRMED**
- ✅ User là thành viên của nhóm **Admins**
- ✅ Profile của user tồn tại trong bảng DynamoDB **UserProfiles**
- ✅ Profile có `userType` được đặt là `"admin"`
- ✅ Tất cả thuộc tính user (email, fullName) được điền đúng

#### Các bước tiếp theo

Bạn đã sẵn sàng để đăng nhập và kiểm tra hệ thống!

{{% notice success %}}
Tài khoản Super Admin của bạn đã sẵn sàng sử dụng. Bây giờ bạn có thể xác thực với API bằng thông tin đăng nhập này.
{{% /notice %}}

{{% notice tip %}}
Giữ thông tin đăng nhập admin an toàn. Cân nhắc sử dụng AWS Secrets Manager cho môi trường production.
{{% /notice %}}
