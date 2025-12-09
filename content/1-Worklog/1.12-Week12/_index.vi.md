---
title: "Worklog Tuần 12"
date: "2025-11-25"
weight: 12
chapter: false
pre: " <b> 1.12. </b> "
---

## Tổng quan

Tuần 12 tập trung vào triển khai WordPress trên AWS — xây dựng một kiến trúc production-ready với EC2 (hoặc Lightsail), RDS cho database (MySQL/MariaDB), cân bằng tải (ALB) và tối ưu phân phối nội dung bằng CloudFront. Nội dung bao gồm cách cấu hình backup, security (Security Groups, IAM), HTTPS (ACM), và dọn dẹp tài nguyên.

### Yêu cầu trước

- Tài khoản AWS có quyền tạo EC2, RDS, ALB, CloudFront, ACM, và S3.
- Biết cơ bản về WordPress, PHP và database.
- `awscli` đã cài nếu muốn thao tác bằng dòng lệnh.

### Mục tiêu

- Triển khai WordPress trên EC2 + RDS hoặc Lightsail (chọn 1 giải pháp) và kiểm tra hoạt động.
- Cấu hình ALB (nếu dùng EC2) để cân bằng tải và enable HTTPS bằng ACM.
- Thiết lập backup: RDS snapshots, periodic EC2 AMI snapshot, và backup media lên S3.
- Tối ưu phân phối nội dung tĩnh bằng CloudFront.
- Dọn dẹp và ghi lại các bước triển khai để tái tạo môi trường.

### Kế hoạch công việc (7 ngày)
| Ngày | Công việc                                                                                                                              | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                                                       |
| ---- | -------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------------ |
| 1    | Lên lựa chọn kiến trúc: EC2+RDS+ALB+CloudFront hoặc Lightsail all-in-one; chuẩn bị AMI/blueprint.                                      | 08/11/2025   | 08/11/2025      | <https://000021.awsstudygroup.com/>                                      |
| 2    | Tạo RDS instance (MySQL), cấu hình subnet group và security group cho phép kết nối từ EC2.                                             | 08/12/2025   | 08/12/2025      | <https://000005.awsstudygroup.com/>, <https://000021.awsstudygroup.com/> |
| 3    | Khởi tạo EC2 instances (hoặc Lightsail), cài LAMP/LEMP, cấu hình PHP và kết nối tới RDS; tạo file wp-config.php và migrate DB nếu cần. | 08/13/2025   | 08/13/2025      | <https://000004.awsstudygroup.com/>, <https://000021.awsstudygroup.com/> |
| 4    | Cấu hình ALB + target group (nếu dùng EC2), cấu hình HTTPS với ACM, và kiểm tra health checks.                                         | 08/14/2025   | 08/14/2025      | <https://000006.awsstudygroup.com/>, <https://000021.awsstudygroup.com/> |
| 5    | Triển khai CloudFront distribution cho nội dung tĩnh (wp-content/uploads) và cấu hình origin access (OAC/OAI) nếu dùng S3.             | 08/15/2025   | 08/15/2025      | <https://000021.awsstudygroup.com/>                                      |
| 6    | Thiết lập backup policy: RDS automated snapshots, tạo AMI cho EC2, sync media lên S3; thử khôi phục từ snapshot.                       | 08/16/2025   | 08/16/2025      | <https://000005.awsstudygroup.com/>, <https://000021.awsstudygroup.com/> |
| 7    | Tối ưu hiệu năng (caching plugin, object cache, CloudFront), dọn dẹp resources không cần thiết, và ghi lại lessons learned.            | 08/17/2025   | 08/17/2025      | <https://cloudjourney.awsstudygroup.com/>                                |

### Kết quả mong đợi

- Website WordPress hoạt động ổn định với database RDS và (nếu cần) ALB + HTTPS.
- Nội dung tĩnh được phục vụ qua CloudFront; backup và snapshot đã được kiểm tra.
- Có tài liệu hướng dẫn triển khai, backup và dọn dẹp tài nguyên.

### Tham chiếu nhanh (CLI ví dụ)

```bash
# Ví dụ: tạo RDS MySQL
aws rds create-db-instance --db-instance-identifier wp-db --db-instance-class db.t3.micro --engine mysql --allocated-storage 20 --master-username admin --master-user-password 'ReplaceMe123!' --db-subnet-group-name my-subnet-group --vpc-security-group-ids sg-01234567 --backup-retention-period 7

# Khởi tạo EC2 để chạy WordPress (ví dụ)
aws ec2 run-instances --image-id ami-0123456789abcdef0 --count 2 --instance-type t3.micro --key-name MyKey --security-group-ids sg-01234567 --subnet-id subnet-aaa

# Tạo ALB và target group (ví dụ cơ bản)
aws elbv2 create-load-balancer --name wp-alb --subnets subnet-aaa subnet-bbb --security-groups sg-01234567
aws elbv2 create-target-group --name wp-targets --protocol HTTP --port 80 --vpc-id vpc-01234567

# Cấp chứng chỉ ACM (request certificate) và attach lên ALB listener (qua console hoặc CLI)
aws acm request-certificate --domain-name example.com --validation-method DNS

# Tạo AMI snapshot cho EC2 (backup)
aws ec2 create-image --instance-id i-0123456789abcdef0 --name "wp-backup-$(date +%Y%m%d)" --no-reboot

# Sync uploads lên S3 (backup media)
aws s3 sync /var/www/html/wp-content/uploads s3://my-wp-backups/uploads/ --delete

# CloudFront: tạo distribution cho S3 origin or ALB origin (cấu hình chi tiết trong console/CLI)
aws cloudfront create-distribution --distribution-config file://cf-config.json

# Dọn dẹp (ví dụ xóa stack test)
aws ec2 terminate-instances --instance-ids i-0123456789abcdef0
aws rds delete-db-instance --db-instance-identifier wp-db --skip-final-snapshot
```

### Tài liệu tham khảo

- WordPress on AWS workshop: <https://000021.awsstudygroup.com/>
- RDS & backup docs: https://docs.aws.amazon.com/rds/
- CloudFront docs: https://docs.aws.amazon.com/cloudfront/

---






