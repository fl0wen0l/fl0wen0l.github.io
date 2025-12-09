---
title: "Cài đặt Dependencies"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

Hệ thống bao gồm hai phần source code cần cài đặt dependencies.

#### Bước 1: Cài đặt cho Backend

Di chuyển đến thư mục Lambda backend và cài đặt các gói Node.js cần thiết:

```bash
cd backend/src/lambda
npm install
```

Lệnh này sẽ cài đặt tất cả dependencies được định nghĩa trong file `package.json` cho Lambda function.

#### Bước 2: Cài đặt cho CDK

Quay lại thư mục gốc CDK và cài đặt dependencies cho CDK:

```bash
# Quay lại thư mục gốc CDK
cd ../../../cdk
npm install
```

Lệnh này cài đặt các thư viện và constructs của AWS CDK cần thiết để synthesize và deploy hạ tầng.

{{% notice tip %}}
Đảm bảo bạn đã cài đặt Node.js và npm trên máy trước khi chạy các lệnh này.
{{% /notice %}}
