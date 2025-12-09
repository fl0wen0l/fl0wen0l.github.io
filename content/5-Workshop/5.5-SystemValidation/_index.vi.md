---
title: "Kiểm tra hệ thống"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

#### Chiến lược kiểm tra

Chúng ta đã triển khai thành công backend và tạo admin user. Bây giờ, chúng ta cần xác thực chức năng của hệ thống.

Chúng ta sẽ mô phỏng một **Client Application** (như ứng dụng React Native) thực hiện các request đến Serverless Backend của bạn.

**Công cụ:** Bạn có thể sử dụng **Postman**, **Thunder Client** (VS Code Extension), hoặc đơn giản là **cURL** trong terminal.

#### Nội dung

- [Lấy cấu hình](5.5.1-retrieve-config/)
- [Test đăng nhập Admin](5.5.2-test-admin-login/)
- [Test tạo Listing](5.5.3-test-create-listing/)
- [Test truy cập công khai](5.5.4-test-public-access/)
- [Xác minh tài nguyên](5.5.5-verify-resources/)
