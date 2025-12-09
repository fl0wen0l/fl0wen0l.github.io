---
title: "Worklog"
date: "2025-12-02"
weight: 1
chapter: false
pre: " <b> 1. </b> "
---

# Nhật ký thực tập — Tóm tắt hàng tuần

Trang nhật ký này ghi lại tiến trình 12 tuần của chương trình thực tập, bao gồm các ghi chú hàng tuần về mục tiêu, nhiệm vụ, kết quả và các lệnh/đoạn mã hữu ích. Bạn có thể truy cập từng liên kết tuần để xem chi tiết: mục tiêu, lệnh CLI, kiểm tra và ghi nhận kết quả.

Hướng dẫn sử dụng
- Bảng dưới đây tóm tắt ngắn gọn nội dung của mỗi tuần (tuần, ngày, chủ đề, điểm nổi bật và liên kết tới trang chi tiết).
- Nhấp vào liên kết của từng tuần để đọc nội dung chi tiết, đoạn mã và các bước xác thực đã thực hiện.

Tổng quan chung
- Thời lượng: 12 tuần
- Trọng tâm: Các khái niệm nền tảng đám mây, mạng, compute, lưu trữ, cơ sở dữ liệu, giám sát, scaling, kết nối hybrid, di cư và triển khai kiểu production.

Tổng quan hàng tuần
| Tuần    | Ngày       | Chủ đề                                            | Điểm nổi bật                                                                                                         | Liên kết                |
| ------- | ---------- | ------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- | ----------------------- |
| Tuần 1  | 2025-08-30 | Bắt đầu — EC2 & CLI                               | Tạo tài khoản AWS, cấu hình CLI, khởi tạo EC2, gắn EBS, thực hành SSH và lệnh CLI cơ bản.                            | [Tuần 1](1.1-Week1/)    |
| Tuần 2  | 2025-08-xx | VPC & Mạng                                        | Thiết kế & triển khai VPC (public/private), cấu hình SGs/NACLs, triển khai VPN Site‑to‑Site và tự động hoá bằng IaC. | [Tuần 2](1.2-Week2/)    |
| Tuần 3  | 2025-08-16 | Hoạt động EC2 & Triển khai ứng dụng               | Khởi tạo instance Linux/Windows, tạo AMI/snapshot, triển khai ứng dụng mẫu, xác thực recovery và truy cập SSM.       | [Tuần 3](1.3-Week3/)    |
| Tuần 4  | 2025-08-23 | IAM Roles & Instance Profile                      | Triển khai quyền truy cập theo vai trò cho EC2, quay vòng khoá tạm thời và áp dụng principle of least privilege.     | [Tuần 4](1.4-Week4/)    |
| Tuần 5  | 2025-08-30 | Cloud9 & Hosting tĩnh (S3)                        | Sử dụng Cloud9 làm môi trường phát triển; host trang tĩnh trên S3, cấu hình CloudFront và versioning.                | [Tuần 5](1.5-Week5/)    |
| Tuần 6  | 2025-09-06 | RDS & Cơ sở dữ liệu quan hệ                       | Triển khai RDS, cấu hình Multi‑AZ và backup, và kiểm tra snapshot/restore.                                           | [Tuần 6](1.6-Week6/)    |
| Tuần 7  | 2025-09-13 | AWS Lightsail                                     | Triển khai WordPress/PrestaShop/Akaunting; sử dụng DB Lightsail, snapshot và tối ưu chi phí.                         | [Tuần 7](1.7-Week7/)    |
| Tuần 8  | 2025-09-20 | Auto Scaling & Load Balancer                      | Tạo Launch Template, ALB và ASG; kiểm tra cơ chế scaling, health-check.                                              | [Tuần 8](1.8-Week8/)    |
| Tuần 9  | 2025-10-04 | Observability — CloudWatch                        | Thu thập metrics/logs, tạo alarm và dashboard; dùng CloudWatch Logs Insights để phân tích.                           | [Tuần 9](1.9-Week9/)    |
| Tuần 10 | 2025-10-11 | Hybrid DNS (Route 53) & AWS CLI                   | Triển khai Route 53 Resolver endpoints; thực hành tự động hoá với AWS CLI và kiểm tra DNS hybrid.                    | [Tuần 10](1.10-Week10/) |
| Tuần 11 | 2025-10-25 | Di cư & Tự động hoá CLI                           | Dùng AWS CLI thu thập inventory, chuẩn bị artifact cho import, thử nghiệm VM import/export và nghiên cứu DMS.        | [Tuần 11](1.11-Week11/) |
| Tuần 12 | 2025-11-01 | Triển khai theo phong cách production (WordPress) | Triển khai WordPress với EC2 + RDS + ALB + ASG; xác thực backup, CloudFront và dọn dẹp.                              | [Tuần 12](1.12-Week12/) |

Kết quả & kỹ năng chính
- Có kinh nghiệm thực hành với các dịch vụ nền tảng AWS và mô hình triển khai serverless và non‑serverless.
- Nâng cao kỹ năng sử dụng AWS CLI, IaC (CDK/Terraform/CloudFormation) và tự động hoá.
- Nắm vững các thao tác giám sát, backup/restore và các thực hành bảo mật cơ bản.



