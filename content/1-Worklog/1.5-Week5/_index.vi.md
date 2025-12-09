---
title: "Worklog Tuần 5"
date: "2025-11-25"
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

## Tổng quan

Tuần 5 tập trung vào môi trường phát triển Cloud9 và triển khai trang tĩnh lên Amazon S3. Bạn sẽ học cách tạo Cloud9 IDE, làm việc trực tiếp trên môi trường Cloud9, chuẩn bị một trang web tĩnh (HTML/CSS/JS), và host nó trên S3 bằng cách cấu hình bucket tĩnh và (tùy chọn) CloudFront để cải thiện hiệu năng.

### Yêu cầu trước

- Tài khoản AWS có quyền tạo Cloud9 environment, S3 bucket và (tuỳ chọn) CloudFront.
- Đã cài `awscli` nếu muốn thao tác từ local hoặc từ Cloud9 terminal.
- Kiến thức cơ bản về HTML/CSS/JavaScript để chuẩn bị trang tĩnh.

### Mục tiêu

- Tạo và sử dụng AWS Cloud9 để phát triển và deploy.
- Chuẩn bị một trang web tĩnh đơn giản và upload lên S3.
- Cấu hình S3 bucket để host trang tĩnh (public read hoặc sử dụng CloudFront + OAI).
- (Tuỳ chọn) Cấu hình CloudFront để cache nội dung và bật HTTPS.
- Ghi chép các bước deploy và dọn dẹp tài nguyên.

### Kế hoạch công việc (7 ngày)
| Ngày | Công việc                                                                                                                     | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                                                       |
| ---- | ----------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------------ |
| 1    | Nghiên cứu Cloud9 và S3 static website hosting; lên kế hoạch nội dung trang tĩnh.                                             | 08/11/2025   | 08/11/2025      | <https://000049.awsstudygroup.com/>, <https://000057.awsstudygroup.com/> |
| 2    | Tạo Cloud9 environment: chọn instance size, VPC/subnet (nếu cần), kiểm tra terminal và quyền IAM.                             | 08/12/2025   | 08/12/2025      | <https://000049.awsstudygroup.com/>                                      |
| 3    | Tạo cấu trúc dự án trang tĩnh (index.html, styles.css, script.js) trên Cloud9; kiểm thử cục bộ.                               | 08/13/2025   | 08/13/2025      | <https://000049.awsstudygroup.com/>                                      |
| 4    | Tạo S3 bucket chuẩn bị host trang tĩnh; cấu hình bucket policy/Permissions (hoặc CloudFront).                                 | 08/14/2025   | 08/14/2025      | <https://000057.awsstudygroup.com/>                                      |
| 5    | Upload files lên S3 (AWS Console / aws cli / s3 sync từ Cloud9); kiểm tra truy cập công khai hoặc thông qua CloudFront.       | 08/15/2025   | 08/15/2025      | <https://000057.awsstudygroup.com/>                                      |
| 6    | (Tuỳ chọn) Thiết lập CloudFront distribution với Origin là S3 bucket (cấu hình OAI/Origin Access Control) và bật HTTPS.       | 08/16/2025   | 08/16/2025      | <https://000057.awsstudygroup.com/>                                      |
| 7    | Dọn dẹp: xóa bucket/objects (hoặc disable cloudfront), terminate Cloud9 environment nếu không cần, tổng kết hướng dẫn deploy. | 08/17/2025   | 08/17/2025      | <https://cloudjourney.awsstudygroup.com/>                                |

### Kết quả mong đợi

- Có một Cloud9 IDE hoạt động cho phát triển web.
- Một trang tĩnh được host trên S3 và có thể truy cập qua URL (hoặc thông qua CloudFront CDN).
- Thành thạo các lệnh `aws s3` cơ bản và thao tác upload/sync từ terminal.
- Viết hướng dẫn dọn dẹp để tránh chi phí phát sinh.

### Tham chiếu nhanh (CLI ví dụ)

```bash
# Tạo bucket (ví dụ, tên bucket phải toàn cầu duy nhất)
aws s3 mb s3://my-static-site-example-$(date +%s)

# Đồng bộ thư mục build lên bucket
aws s3 sync ./site/ s3://my-static-site-example-123 --delete

# Thiết lập quyền public-read (không khuyến nghị cho production; dùng CloudFront + OAC thay vì public)
aws s3api put-bucket-policy --bucket my-static-site-example-123 --policy file://bucket-policy.json

# Liệt kê objects
aws s3 ls s3://my-static-site-example-123

# Xoá objects (cleanup)
aws s3 rm s3://my-static-site-example-123 --recursive

# Remove bucket
aws s3 rb s3://my-static-site-example-123 --force
```

### Tài liệu tham khảo

- AWS Cloud9 workshop: <https://000049.awsstudygroup.com/>
- S3 static website hosting: <https://000057.awsstudygroup.com/>
- CloudFront & S3 best practices: https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html

---





