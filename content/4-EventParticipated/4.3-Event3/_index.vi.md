---
title: "Event 3"
date: "2025-11-29"
weight: 3
chapter: false
pre: " <b> 4.3. </b> "
---

**AWS Cloud Mastery Series #3 — Theo AWS Well-Architected: Security Pillar**
 
Thời gian: **Saturday, 29 November 2025 | 08:30 - 12:00 (GMT+7)**

Địa điểm: **Bitexco Financial Tower, Quận 1, TP. HCM**


## Cảm nghĩ cá nhân sau buổi "Security Pillar"

Buổi chia sẻ theo khuôn khổ **AWS Well‑Architected — Security Pillar** mang lại cho tôi một bức tranh hệ thống về an ninh trên cloud, từ nhận thức chiến lược đến các kỹ thuật thực thi cụ thể. Nội dung được trình bày theo 5 mảng chính (IAM, Detection, Infrastructure Protection, Data Protection, Incident Response) khiến tôi dễ nắm bắt và liên hệ với thực tế vận hành.

### Những kiến thức chính tôi tiếp thu

- Identity & Access Management (IAM): Tôi được nhắc lại các nguyên tắc tránh dùng long‑term credentials, ưu tiên role‑based access và SSO qua IAM Identity Center; khái niệm SCP và permission boundaries cho multi‑account giúp tôi hình dung cách phân tầng quyền hạn an toàn cho tổ chức lớn.
- Detection & Logging: Việc thiết kế logging ở mọi tầng (CloudTrail ở org‑level, VPC Flow Logs, ALB/S3 logs) và tích hợp GuardDuty + Security Hub giúp phát hiện bất thường sớm. Khái niệm Detection‑as‑Code và tự động hoá rule với EventBridge là hướng tiếp cận rất thực tế.
- Network & Workload Security: Phân đoạn VPC, đặt workload cần thiết vào private subnet, sử dụng Security Groups + NACL đúng mục đích, và áp dụng WAF/Shield cho lớp ứng dụng — các ví dụ demo cho thấy cách giảm mặt phẳng tấn công.
- Data Protection: Cốt lõi là mã hoá (KMS) với key policies/grants, rotation và pattern sử dụng Secrets Manager/Parameter Store để quản lý bí mật; tôi hiểu rõ hơn về encryption at‑rest và in‑transit cho S3, EBS, RDS.
- Incident Response (IR): Playbook cho các kịch bản (compromised IAM key, S3 public exposure, EC2 malware) và quy trình snapshot/isolate/evidence collection; sử dụng Lambda/Step Functions để tự động hóa phản hồi giúp giảm thời gian phản ứng.

### Những điểm ấn tượng & hữu dụng

- Tính thực tiễn của các playbook IR: không chỉ là lý thuyết mà có bước thực thi rõ ràng (snapshot, isolate, forensic). Điều này rất hữu dụng khi team cần có SOP cho sự cố.
- Detection‑as‑Code: Việc lưu cấu hình detection và rule dưới dạng mã giúp tái sử dụng, review và test giống như mã ứng dụng — đây là bước tiến để nâng maturity cho đội security.
- Các ví dụ về thiết kế IAM cho multi‑account thực sự giúp tôi hiểu cách áp dụng least privilege ở quy mô doanh nghiệp.

### Ứng dụng ngay vào công việc của tôi

1. Bắt đầu audit nhỏ: rà soát IAM roles/policies hiện có, xoá long‑term credentials và bật MFA cho các account quan trọng.
2. Thiết lập CloudTrail ở mức tổ chức (org‑level) nếu chưa có, đồng thời bổ sung log forwarding vào một account bảo mật để phân tích và lưu trữ.
3. Viết một playbook IR cơ bản cho 2 kịch bản: (a) compromised key, (b) S3 public exposure — kèm checklist thao tác và snapshot/restore steps.
4. Tạo một proof‑of‑concept nhỏ cho detection-as-code: lưu rule GuardDuty/CloudWatch Event vào repo, test deploy qua CI.

### Kết luận

Buổi workshop giúp tôi hiểu rõ rằng security là một hành trình: cần có governance (policies, least privilege), observability (logs, detection), bảo vệ lớp nền tảng (network, workload, data), và khả năng phản ứng tự động. Tôi có một danh sách hành động cụ thể để bắt đầu cải thiện posture cho các dự án nhỏ trong 2–4 tuần tới.

Nếu bạn muốn, tôi có thể chuyển phần này thành slide 1 trang hoặc checklist IR ngắn gọn để nộp báo cáo.

