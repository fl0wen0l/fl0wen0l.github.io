---
title: "Tạo dữ liệu khởi tạo"
date: "`r Sys.Date()`"
weight: 4
chapter: false
pre: " <b> 5.4. </b> "
---

#### Tại sao cần seed data?

Hạ tầng của bạn đã sẵn sàng, nhưng cơ sở dữ liệu còn trống. Bạn cần một tài khoản **"Super Admin"** để quản lý hệ thống.

Chúng ta sẽ chạy một script Node.js kết nối trực tiếp đến AWS để:

- Tạo User trong Cognito User Pool
- Thêm user đó vào Admins Group
- Tạo profile tương ứng trong bảng DynamoDB UserProfiles

#### Nội dung

- [Thiết lập Script](5.4.1-setup-script/)
- [Cấu hình môi trường](5.4.2-configure-environment/)
- [Chạy Script](5.4.3-run-script/)
- [Xác minh](5.4.4-verification/)
