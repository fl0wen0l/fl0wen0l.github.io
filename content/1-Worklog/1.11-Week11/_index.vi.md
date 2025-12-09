---
title: "Worklog Tuần 11"
date: "2025-11-25"
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---

## Tổng quan

Tuần 11 kết hợp hai chủ đề chính: **thao tác và tự động hoá bằng AWS CLI** và **tổng quan các chiến lược di trú (migration)**. Mục tiêu là thành thạo việc dùng CLI để quản lý tài nguyên nhanh chóng, viết script deploy/cleanup, và hiểu các phương pháp di trú (lift-and-shift, replatform, database migration — DMS, VM import/export, v.v.) để lên kế hoạch chuyển đổi hệ thống thực tế.

### Yêu cầu trước

- Tài khoản AWS có quyền sử dụng các dịch vụ liên quan (EC2, S3, DMS, IAM, v.v.).
- Đã cài `awscli` và cấu hình profile cơ bản (Access Key, Secret, region).
- Kiến thức nền tảng về mạng, máy chủ, và cơ sở dữ liệu giúp khi thực hành migration.

### Mục tiêu

- Nắm vững lệnh AWS CLI thông dụng: `aws configure`, `describe`, `create`, `delete`, `--query`, `--profile`, `--output`.
- Viết script tự động hoá deploy và cleanup (bash / PowerShell) sử dụng CLI.
- Hiểu các mẫu di trú: S3 transfer, VM Import/Export, Database Migration Service (DMS) — ưu/nhược điểm và workflow cơ bản.
- Thực hành một kịch bản migration nhỏ (ví dụ: sync dữ liệu S3 hoặc khôi phục DB từ snapshot) và ghi lại checklist.

### Kế hoạch công việc (7 ngày)
| Ngày | Công việc                                                                                                                                       | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                                 |
| ---- | ----------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | -------------------------------------------------- |
| 1    | Học lại cơ bản AWS CLI: cài đặt, `aws configure`, quản lý profiles, dùng `--query` và `--output` để lọc kết quả.                                | 08/11/2025   | 08/11/2025      | <https://000011.awsstudygroup.com/>                |
| 2    | Thực hành các lệnh quản lý EC2/S3/ IAM qua CLI: launch instance, s3 sync, tạo policy, v.v.                                                      | 08/12/2025   | 08/12/2025      | <https://000011.awsstudygroup.com/>                |
| 3    | Nghiên cứu các chiến lược di trú: lift-and-shift, replatform, database migration (DMS), VM Import/Export. Lên kế hoạch migration mẫu.           | 08/13/2025   | 08/13/2025      | <https://cloudjourney.awsstudygroup.com/2-migrate> |
| 4    | Thực hành S3 migration: dùng `aws s3 sync` để chuyển dữ liệu, kiểm tra checksum và cấu hình bucket phiên bản/backup.                            | 08/14/2025   | 08/14/2025      | <https://cloudjourney.awsstudygroup.com/>          |
| 5    | Thực hành Database Migration Service (giới thiệu): tạo replication instance, source/target endpoints, và replication task (theo dõi migration). | 08/15/2025   | 08/15/2025      | https://docs.aws.amazon.com/dms/                   |
| 6    | Thực hành VM Import/Export (nếu có VM image): chuẩn bị VM, upload tới S3, import image, kiểm tra instance khởi động.                            | 08/16/2025   | 08/16/2025      | https://docs.aws.amazon.com/vm-import/             |
| 7    | Viết script deploy/cleanup (bash/PowerShell), test toàn bộ flow migration nhỏ, và dọn dẹp tài nguyên. Ghi lại lessons learned.                  | 08/17/2025   | 08/17/2025      | <https://cloudjourney.awsstudygroup.com/>          |

### Kết quả mong đợi

- Thành thạo các lệnh AWS CLI cơ bản để quản lý và tự động hoá thao tác.
- Có kịch bản migration nhỏ đã thực hiện (ví dụ sync S3 hoặc restore DB), với các bước kiểm tra và dọn dẹp.
- Hiểu khi nào sử dụng DMS, VM Import/Export, hoặc các phương án khác tuỳ theo trường hợp.

### Tham chiếu nhanh (CLI ví dụ)

```bash
# Cấu hình CLI
aws configure --profile workshop

# Liệt kê instances
aws ec2 describe-instances --query 'Reservations[*].Instances[*].[InstanceId,State.Name,InstanceType]' --output table --profile workshop

# Đồng bộ thư mục local lên S3
aws s3 sync ./data/ s3://my-migration-bucket/data/ --profile workshop

# Ví dụ: upload VM image cho VM Import (bước upload lên S3)
aws s3 cp MyVM-image.vmdk s3://my-import-bucket/myvm.vmdk --profile workshop

# VM Import: tạo import-image task (ví dụ)
aws ec2 import-image --description "My VM" --disk-containers "file://containers.json" --profile workshop

# DMS: tạo replication instance (ví dụ - tuỳ thuộc vào cấu hình)
aws dms create-replication-instance --replication-instance-identifier my-dms-instance --allocated-storage 50 --replication-instance-class dms.t3.medium --engine-version 3.4.6

# DMS: tạo endpoints (source/target) và replication task (chi tiết tham khảo docs)

# Ví dụ script cleanup (xóa bucket content)
aws s3 rm s3://my-migration-bucket/data/ --recursive --profile workshop

# Dùng --dry-run pattern (nhiều lệnh s3/cli không có dry-run, so be careful)

```

### Tài liệu tham khảo

- AWS CLI workshop: <https://000011.awsstudygroup.com/>
- CloudJourney migration overview: <https://cloudjourney.awsstudygroup.com/2-migrate>
- DMS docs: https://docs.aws.amazon.com/dms/
- VM Import/Export docs: https://docs.aws.amazon.com/vm-import/

---






