---
title: "Thiết lập Script"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 5.4.1. </b> "
---

#### Bước 1: Di chuyển đến thư mục

Di chuyển đến thư mục scripts nơi chứa script seeding:

```bash
cd backend/scripts
```

#### Bước 2: Cài đặt dependencies

Script sử dụng **AWS SDK v3**. Cài đặt các gói cần thiết:

```bash
npm install
```

Lệnh này sẽ cài đặt tất cả dependencies được định nghĩa trong file `package.json`, bao gồm:

- `@aws-sdk/client-cognito-identity-provider` - Cho các thao tác Cognito
- `@aws-sdk/client-dynamodb` - Cho các thao tác DynamoDB
- `dotenv` - Để quản lý biến môi trường
- Các dependencies khác cần thiết

{{% notice info %}}
Đảm bảo bạn đang ở thư mục `backend/scripts` trước khi chạy `npm install`.
{{% /notice %}}

Đợi quá trình cài đặt hoàn tất thành công trước khi chuyển sang bước tiếp theo.
