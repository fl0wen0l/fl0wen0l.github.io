---
title: "Worklog Tuần 3"
date: "2025-11-25"
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

## Tổng quan

Tuần 3 tập trung vào Amazon EC2 — cách chọn AMI và instance type, tạo và quản lý instances, gắn ổ đĩa EBS, cấu hình key pair và security groups, sử dụng IAM role cho instance, tạo AMI snapshot và thực hành dọn dẹp tài nguyên.

### Yêu cầu trước

- Có tài khoản AWS với quyền tạo EC2, EBS, IAM.
- Đã cài `awscli` và cấu hình access key/secret và region.
- Đã tạo một key pair (hoặc biết cách tạo) để SSH vào instance.

### Mục tiêu

- Hiểu kiến trúc và thành phần của EC2 (AMI, instance type, EBS, Elastic IP).
- Triển khai instance EC2, kết nối SSH, gắn/detach EBS.
- Tạo AMI từ một instance cấu hình sẵn và snapshot EBS.
- Sử dụng IAM Role gán cho EC2 để truy cập S3/CloudWatch an toàn.
- Ghi chép bước triển khai và dọn dẹp tài nguyên sau thực hành.

### Kế hoạch công việc (7 ngày)
| Ngày | Công việc                                                                                                                | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                        |
| ---- | ------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | ----------------------------------------- |
| 1    | Tìm hiểu khái niệm: AMI, instance type, EBS, key pair, security group, Elastic IP. Lên kế hoạch cấu hình lab.            | 08/11/2025   | 08/11/2025      | <https://000004.awsstudygroup.com/>       |
| 2    | Tạo key pair (nếu chưa có), tạo security group cho SSH/HTTP, chuẩn bị subnet/VPC cho instance.                           | 08/12/2025   | 08/12/2025      | <https://000004.awsstudygroup.com/>       |
| 3    | Khởi tạo EC2 instance: chọn AMI, instance type, gắn key pair và security group. Kiểm tra trạng thái running.             | 08/13/2025   | 08/13/2025      | <https://000004.awsstudygroup.com/>       |
| 4    | Kết nối SSH vào instance, thực hiện cấu hình cơ bản (cập nhật hệ thống, cài đặt web server nếu cần).                     | 08/14/2025   | 08/14/2025      | <https://000004.awsstudygroup.com/>       |
| 5    | Thực hành gắn EBS: tạo volume, attach vào instance, format và mount. Tạo snapshot của volume khi cần.                    | 08/15/2025   | 08/15/2025      | <https://000004.awsstudygroup.com/>       |
| 6    | Tạo AMI từ instance đã cấu hình; kiểm tra khởi động từ AMI mới. Gán IAM Role nếu cần truy cập S3.                        | 08/16/2025   | 08/16/2025      | <https://000048.awsstudygroup.com/>       |
| 7    | Dọn dẹp: stop/terminate instance, xóa volumes/AMIs nếu không cần, tổng kết bài học và ghi chú chi tiết các lệnh đã dùng. | 08/17/2025   | 08/17/2025      | <https://cloudjourney.awsstudygroup.com/> |

### Kết quả mong đợi

- Khởi tạo và kết nối thành công với EC2 instance.
- Gắn và quản lý EBS volume; thực hiện snapshot và khôi phục đơn giản.
- Tạo AMI từ instance đã cấu hình để dùng lại.
- Hiểu cách gán IAM Role cho EC2 để truy cập tài nguyên AWS an toàn.
- Viết lại các lệnh CLI quan trọng và lưu trữ hướng dẫn dọn dẹp.

### Tham chiếu nhanh (CLI ví dụ)

```bash
# Kiểm tra instances
aws ec2 describe-instances --query 'Reservations[*].Instances[*].[InstanceId,State.Name,InstanceType,PublicIpAddress]' --output table

# Khởi tạo instance (ví dụ)
aws ec2 run-instances --image-id ami-0123456789abcdef0 --count 1 --instance-type t3.micro --key-name MyKey --security-group-ids sg-01234567 --subnet-id subnet-01234567

# Tạo EBS volume
aws ec2 create-volume --availability-zone us-east-1a --size 10 --volume-type gp3

# Attach volume
aws ec2 attach-volume --volume-id vol-0123456789abcdef0 --instance-id i-0123456789abcdef0 --device /dev/sdf

# Tạo AMI từ instance
aws ec2 create-image --instance-id i-0123456789abcdef0 --name "my-server-ami-$(date +%Y%m%d)" --no-reboot

# Stop / Terminate instance
aws ec2 stop-instances --instance-ids i-0123456789abcdef0
aws ec2 terminate-instances --instance-ids i-0123456789abcdef0
```

### Tài liệu tham khảo

- EC2 workshop: <https://000004.awsstudygroup.com/>
- IAM Roles for EC2: <https://000048.awsstudygroup.com/>
- CloudJourney overview: <https://cloudjourney.awsstudygroup.com/>

---






