---
title: "Worklog Tuần 7"
date: "2025-11-25"
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

## Tổng quan

Tuần 7 tập trung vào Amazon Lightsail — dịch vụ đơn giản hoá việc triển khai ứng dụng (ví dụ WordPress, LAMP) với chi phí và cấu hình dễ quản lý. Bạn sẽ học cách tạo instance Lightsail, gắn disk, cấu hình networking, thực hiện snapshot, backup và dọn dẹp tài nguyên.

### Yêu cầu trước

- Tài khoản AWS có quyền sử dụng Lightsail.
- Kiến thức cơ bản về tạo và quản lý instance (SSH, key pair).
- `awscli` tuỳ chọn (Lightsail cũng hỗ trợ CLI riêng `aws lightsail`).

### Mục tiêu

- Hiểu khi nào dùng Lightsail thay vì EC2.
- Tạo và cấu hình Lightsail instance (chọn blueprint hoặc deploy WordPress/LAMP).
- Quản lý disk, snapshot và public IP; cấu hình DNS (Lightsail DNS hoặc Route53).
- Thực hiện backup/snapshot và dọn dẹp resources sau thực hành.

### Kế hoạch công việc (7 ngày)
| Ngày | Công việc                                                                                               | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                        |
| ---- | ------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 1    | Nghiên cứu khái niệm Lightsail và so sánh với EC2 (khi nào nên dùng Lightsail).                         | 08/11/2025   | 08/11/2025      | <https://000045.awsstudygroup.com/>       |
| 2    | Tạo Lightsail instance: chọn blueprint (WordPress/Node/LAMP), chọn plan (memory/cpu), cấu hình SSH key. | 08/12/2025   | 08/12/2025      | <https://000045.awsstudygroup.com/>       |
| 3    | Cấu hình ứng dụng: truy cập SSH, cấu hình web server, cài đặt plugin (WordPress) hoặc deploy app.       | 08/13/2025   | 08/13/2025      | <https://000045.awsstudygroup.com/>       |
| 4    | Gắn/định cấu hình block storage nếu cần, tạo snapshot/backup ban đầu.                                   | 08/14/2025   | 08/14/2025      | <https://000045.awsstudygroup.com/>       |
| 5    | Thiết lập DNS (Lightsail DNS) hoặc dùng Route53; cấu hình static IP và kiểm tra truy cập.               | 08/15/2025   | 08/15/2025      | <https://000045.awsstudygroup.com/>       |
| 6    | Kiểm thử: snapshot/restore, kiểm tra performance, backup policy; note chi phí và giới hạn.              | 08/16/2025   | 08/16/2025      | <https://000045.awsstudygroup.com/>       |
| 7    | Dọn dẹp: delete instance/snapshots/static IP (nếu không cần), ghi lại lessons learned.                  | 08/17/2025   | 08/17/2025      | <https://cloudjourney.awsstudygroup.com/> |

### Kết quả mong đợi

- Triển khai thành công một ứng dụng trên Lightsail (ví dụ WordPress) và truy cập qua static IP hoặc domain.
- Thực hành snapshot/restore và quản lý disk.
- Biết cách cấu hình DNS và (nếu cần) tích hợp với Route53.
- Có checklist dọn dẹp để tránh chi phí phát sinh.

### Tham chiếu nhanh (CLI ví dụ)

```bash
# Liệt kê instances Lightsail
aws lightsail get-instances

# Tạo instance Lightsail từ blueprint WordPress
aws lightsail create-instances --instance-names my-wp-site --availability-zone us-east-1a --blueprint-id wordpress --bundle-id nano_2_0

# Lấy public IP của instance
aws lightsail get-instance --instance-name my-wp-site --query 'instance.publicIpAddress'

# Tạo snapshot
aws lightsail create-instance-snapshot --instance-name my-wp-site --instance-snapshot-name my-wp-snap-$(date +%Y%m%d)

# Đính kèm block storage (nếu cần)
aws lightsail create-disk --disk-name my-disk --size-in-gb 20 --availability-zone us-east-1a
aws lightsail attach-disk --disk-name my-disk --instance-name my-wp-site --disk-location /dev/xvdf

# Xóa instance (cleanup)
aws lightsail delete-instance --instance-name my-wp-site
```

### Tài liệu tham khảo

- Lightsail workshop: <https://000045.awsstudygroup.com/>
- Lightsail docs: https://docs.aws.amazon.com/lightsail/
- CloudJourney overview: <https://cloudjourney.awsstudygroup.com/>





