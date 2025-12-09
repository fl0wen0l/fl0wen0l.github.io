---
title: "Worklog Tuần 4"
date: "2025-11-25"
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

## Tổng quan

Tuần 4 tập trung vào IAM Roles cho EC2: hiểu cách cấp quyền cho instance thông qua IAM Role (Instance Profile), áp dụng nguyên tắc least-privilege, và cấu hình role để EC2 truy cập các dịch vụ AWS (ví dụ: S3, CloudWatch) mà không cần nhúng credential tĩnh.

### Yêu cầu trước

- Có tài khoản AWS với quyền tạo IAM Role và EC2.
- Hiểu cơ bản về EC2 và AWS CLI (`awscli` đã cài và cấu hình).

### Mục tiêu

- Hiểu khác biệt giữa IAM User, IAM Role, và Instance Profile.
- Tạo IAM Role với trust policy cho EC2 và attach các managed/custom policy phù hợp.
- Gán Role (Instance Profile) cho EC2 và kiểm tra quyền truy cập (ví dụ: đọc/ghi S3, ghi log lên CloudWatch).
- Thực hành nguyên tắc least privilege và quản lý permission.

### Kế hoạch công việc (7 ngày)
| Ngày | Công việc                                                                                                                                                 | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                        |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 1    | Nghiên cứu lý thuyết: IAM User vs Role vs Instance Profile, trust policy, managed policy vs inline policy.                                                | 08/11/2025   | 08/11/2025      | <https://000048.awsstudygroup.com/>       |
| 2    | Viết trust policy cho EC2 (cho phép service `ec2.amazonaws.com` assume role). Soạn policy mẫu cho truy cập S3 (read-only) và CloudWatch (put metric/log). | 08/12/2025   | 08/12/2025      | <https://000048.awsstudygroup.com/>       |
| 3    | Tạo IAM Role bằng CLI/Console; attach policy managed hoặc custom; tạo Instance Profile.                                                                   | 08/13/2025   | 08/13/2025      | <https://000048.awsstudygroup.com/>       |
| 4    | Khởi tạo EC2 hoặc gán role cho EC2 hiện có; kiểm tra metadata service và quyền (ví dụ `aws s3 ls`).                                                       | 08/14/2025   | 08/14/2025      | <https://000048.awsstudygroup.com/>       |
| 5    | Thực hành least-privilege: sửa policy để giảm scope (prefix/resource ARNs), test chức năng.                                                               | 08/15/2025   | 08/15/2025      | <https://000048.awsstudygroup.com/>       |
| 6    | Thử kịch bản: EC2 upload file lên S3, ghi log lên CloudWatch Logs/metrics bằng IAM Role. Ghi lại các bước và kết quả.                                     | 08/16/2025   | 08/16/2025      | <https://000048.awsstudygroup.com/>       |
| 7    | Dọn dẹp: xóa role/instance-profile (nếu không cần), xóa test data trên S3, ghi bài học và đề xuất bảo mật.                                                | 08/17/2025   | 08/17/2025      | <https://cloudjourney.awsstudygroup.com/> |

### Kết quả mong đợi

- Tạo và gán thành công IAM Role cho EC2 (Instance Profile).
- EC2 có thể tương tác với S3/CloudWatch mà không cần Access Key tĩnh.
- Policies được cấu hình theo nguyên tắc least privilege đáp ứng yêu cầu bài tập.
- Tài liệu hướng dẫn các bước triển khai và dọn dẹp hoàn chỉnh.

### Tham chiếu nhanh (CLI ví dụ)

```bash
# Tạo trust policy file (trust-policy.json)
cat > trust-policy.json << 'EOF'
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Principal": {"Service": "ec2.amazonaws.com"},
			"Action": "sts:AssumeRole"
		}
	]
}
EOF

# Tạo role
aws iam create-role --role-name EC2S3ReadOnlyRole --assume-role-policy-document file://trust-policy.json

# Gắn managed policy (ví dụ AmazonS3ReadOnlyAccess)
aws iam attach-role-policy --role-name EC2S3ReadOnlyRole --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess

# Tạo instance profile và add role
aws iam create-instance-profile --instance-profile-name EC2S3ReadOnlyProfile
aws iam add-role-to-instance-profile --instance-profile-name EC2S3ReadOnlyProfile --role-name EC2S3ReadOnlyRole

# Khởi tạo EC2 với instance profile (ví dụ)
aws ec2 run-instances --image-id ami-0123456789abcdef0 --count 1 --instance-type t3.micro --iam-instance-profile Name=EC2S3ReadOnlyProfile --key-name MyKey --subnet-id subnet-01234567

# Trên EC2, kiểm tra quyền (ví dụ):
aws s3 ls s3://your-test-bucket

# Dọn dẹp: detach và xoá role / instance profile (cẩn thận, đảm bảo không bị ràng buộc)
aws iam remove-role-from-instance-profile --instance-profile-name EC2S3ReadOnlyProfile --role-name EC2S3ReadOnlyRole
aws iam delete-instance-profile --instance-profile-name EC2S3ReadOnlyProfile
aws iam detach-role-policy --role-name EC2S3ReadOnlyRole --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess
aws iam delete-role --role-name EC2S3ReadOnlyRole
```

### Tài liệu tham khảo

- IAM Roles for EC2 workshop: <https://000048.awsstudygroup.com/>
- IAM docs: https://docs.aws.amazon.com/iam/
- CloudJourney overview: <https://cloudjourney.awsstudygroup.com/>





