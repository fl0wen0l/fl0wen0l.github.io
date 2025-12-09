---
title: "Worklog Tuần 6"
date: "2025-11-25"
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

## Tổng quan

Tuần 6 tập trung vào Amazon RDS — quản trị cơ sở dữ liệu được quản lý trên AWS (ví dụ: MySQL, PostgreSQL). Bạn sẽ học cách tạo DB instance, cấu hình subnet group, security group, backup & snapshot, Multi-AZ, read replica, và các thao tác restore/backup. Đồng thời học cách dọn dẹp tài nguyên an toàn.

### Yêu cầu trước

- Tài khoản AWS có quyền tạo RDS instances, subnet groups và security groups.
- Hiểu cơ bản về mạng (VPC/subnet/Security Group) và EC2 để kiểm tra kết nối.
- `awscli` đã cài và cấu hình (tuỳ chọn dùng Console).

### Mục tiêu

- Hiểu các khái niệm: DB instance, DB subnet group, parameter group, option group, Multi-AZ, read replica.
- Tạo DB instance RDS (ví dụ MySQL), thiết lập backup và maintenance window.
- Thực hiện snapshot backup và khôi phục từ snapshot.
- Kiểm tra cấu hình bảo mật (security group), backup retention và monitoring.
- Dọn dẹp tài nguyên (delete instance, snapshots) để tránh chi phí phát sinh.

### Kế hoạch công việc (7 ngày)
| Ngày | Công việc                                                                                                                                      | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                        |
| ---- | ---------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 1    | Nghiên cứu khái niệm RDS và lựa chọn engine (MySQL/Postgres). Lên kế hoạch cấu hình (instance class, storage, Multi-AZ hay không).             | 08/11/2025   | 08/11/2025      | <https://000005.awsstudygroup.com/>       |
| 2    | Tạo DB subnet group (nêu rõ subnets private) và security group cho phép kết nối từ ứng dụng/EC2.                                               | 08/12/2025   | 08/12/2025      | <https://000005.awsstudygroup.com/>       |
| 3    | Tạo DB instance RDS (ví dụ MySQL) với backup retention và enable storage encryption nếu cần. Kiểm tra trạng thái available.                    | 08/13/2025   | 08/13/2025      | <https://000005.awsstudygroup.com/>       |
| 4    | Kết nối thử từ EC2 (hoặc từ client có quyền), tạo bảng mẫu và chèn dữ liệu kiểm thử.                                                           | 08/14/2025   | 08/14/2025      | <https://000005.awsstudygroup.com/>       |
| 5    | Tạo snapshot thủ công, thử restore sang instance mới. (Test restore workflow).                                                                 | 08/15/2025   | 08/15/2025      | <https://000005.awsstudygroup.com/>       |
| 6    | Thử cấu hình read replica (nếu engine hỗ trợ) hoặc kích hoạt Multi-AZ; kiểm tra replication/latency. Kiểm tra monitoring (CloudWatch metrics). | 08/16/2025   | 08/16/2025      | <https://000005.awsstudygroup.com/>       |
| 7    | Dọn dẹp: xóa DB instance (với hoặc không snapshot tùy chọn), xóa snapshots không cần thiết, ghi lại bài học và các lệnh đã dùng.               | 08/17/2025   | 08/17/2025      | <https://cloudjourney.awsstudygroup.com/> |

### Kết quả mong đợi

- Tạo thành công một RDS DB instance và kết nối đến nó từ EC2/client.
- Tạo snapshot backup và khôi phục dữ liệu từ snapshot.
- Hiểu và (nếu thực hành) cấu hình read replica hoặc Multi-AZ cho tính sẵn sàng và scale-read.
- Có quy trình dọn dẹp tài nguyên và lưu trữ các lệnh CLI thường dùng.

### Tham chiếu nhanh (CLI ví dụ)

```bash
# Tạo DB subnet group
aws rds create-db-subnet-group --db-subnet-group-name my-subnet-group --db-subnet-group-description "My subnet group" --subnet-ids subnet-aaa subnet-bbb

# Tạo DB instance (MySQL ví dụ)
aws rds create-db-instance --db-instance-identifier mydbinstance --db-instance-class db.t3.micro --engine mysql --allocated-storage 20 --master-username admin --master-user-password 'ReplaceMe123!' --db-subnet-group-name my-subnet-group --vpc-security-group-ids sg-01234567 --backup-retention-period 7

# Kiểm tra trạng thái
aws rds describe-db-instances --db-instance-identifier mydbinstance

# Tạo snapshot thủ công
aws rds create-db-snapshot --db-snapshot-identifier mydb-snapshot-$(date +%Y%m%d) --db-instance-identifier mydbinstance

# Khôi phục từ snapshot
aws rds restore-db-instance-from-db-snapshot --db-instance-identifier mydb-restored --db-snapshot-identifier mydb-snapshot-20250101

# Tạo read replica (MySQL)
aws rds create-db-instance-read-replica --db-instance-identifier mydb-replica --source-db-instance-identifier mydbinstance --db-instance-class db.t3.micro

# Xoá DB instance (với tạo final snapshot nếu cần)
aws rds delete-db-instance --db-instance-identifier mydbinstance --skip-final-snapshot
```

### Tài liệu tham khảo

- RDS workshop: <https://000005.awsstudygroup.com/>
- RDS docs: https://docs.aws.amazon.com/rds/
- CloudJourney overview: <https://cloudjourney.awsstudygroup.com/>




