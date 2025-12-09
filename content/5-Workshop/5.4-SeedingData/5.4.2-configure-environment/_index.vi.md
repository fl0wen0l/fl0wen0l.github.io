---
title: "Cấu hình môi trường"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 5.4.2. </b> "
---

Script (`seed-admin.js`) cần các biến môi trường cụ thể để kết nối đến các tài nguyên AWS của bạn. Bạn cần tạo file `.env` với các giá trị từ CDK Deployment Output.

#### Tạo file .env

Tạo file có tên `.env` trong thư mục `backend/scripts` với nội dung sau:

```bash
# Nội dung file .env
REGION=us-east-1
USER_POOL_ID=<Paste_Value_From_CDK_Output>
USER_PROFILES_TABLE_NAME=UserProfiles
```

#### Giải thích các biến môi trường

| Biến                         | Mô tả                                     | Tìm ở đâu                                                                                                                 |
| ---------------------------- | ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| **REGION**                   | AWS region nơi tài nguyên được triển khai | `us-east-1` (hoặc `ap-southeast-1` nếu bạn đã thay đổi)                                                                   |
| **USER_POOL_ID**             | Định danh Cognito User Pool               | Sao chép giá trị của `BackendStack.UserPoolId` từ terminal sau khi chạy `cdk deploy`                                      |
| **USER_PROFILES_TABLE_NAME** | Tên bảng DynamoDB cho user profiles       | Trong code CDK, chúng ta đặt tên bảng là `UserProfiles`. Nếu bạn thay đổi, kiểm tra DynamoDB Console để lấy tên chính xác |

#### Ví dụ cấu hình

```bash
# Ví dụ file .env
REGION=us-east-1
USER_POOL_ID=us-east-1_AbCdEfGh
USER_PROFILES_TABLE_NAME=UserProfiles
```

{{% notice warning %}}
Đảm bảo thay thế `<Paste_Value_From_CDK_Output>` bằng giá trị thực tế từ outputs của CDK deployment!
{{% /notice %}}

{{% notice tip %}}
Bạn có thể tìm CDK outputs bằng cách cuộn lên terminal hoặc chạy lại `cdk deploy` (nó sẽ không triển khai lại nếu không có gì thay đổi).
{{% /notice %}}
