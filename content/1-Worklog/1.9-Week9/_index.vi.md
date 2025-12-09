---
title: "Worklog Tuần 9"
date: "2025-11-25"
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

## Tổng quan

Tuần 9 tập trung vào Amazon CloudWatch — dịch vụ giám sát và observability của AWS. Trong tuần này bạn sẽ tìm hiểu cách thu thập metrics, logs, tạo alarms, dashboard và sử dụng CloudWatch Logs Insights để phân tích logs; đồng thời cấu hình hành động (SNS/Auto Scaling) khi alarm kích hoạt.

### Yêu cầu trước

- Tài khoản AWS có quyền xem/ tạo CloudWatch metrics, logs và alarms.
- Có ứng dụng/EC2/ALB tạo ra logs hoặc sẵn sàng gửi custom metrics.
- `awscli` đã cài để thực hành các lệnh CLI nếu cần.

### Mục tiêu

- Hiểu cấu trúc CloudWatch: metrics, logs, events, alarms, dashboards.
- Thu thập và hiển thị metrics (EC2/ALB/RDS) và gửi custom metrics.
- Thiết lập CloudWatch Logs cho ứng dụng và sử dụng Logs Insights để truy vấn.
- Tạo alarms và kết nối action (SNS topic) để nhận thông báo hoặc hành động tự động.
- Xây dựng dashboard tóm tắt các chỉ số quan trọng.

### Kế hoạch công việc (7 ngày)
| Ngày | Công việc                                                                                                                                        | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                        |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------ | ------------ | --------------- | ----------------------------------------- |
| 1    | Nghiên cứu các khái niệm trong CloudWatch: metrics, logs, alarms, dashboards, Logs Insights.                                                     | 08/11/2025   | 08/11/2025      | <https://000008.awsstudygroup.com/>       |
| 2    | Kiểm tra metrics có sẵn cho EC2/ALB/RDS; tìm hiểu namespace và statistic types.                                                                  | 08/12/2025   | 08/12/2025      | <https://000008.awsstudygroup.com/>       |
| 3    | Cấu hình CloudWatch Logs: gửi application logs từ EC2/CloudWatch agent hoặc ALB access logs.                                                     | 08/13/2025   | 08/13/2025      | <https://000008.awsstudygroup.com/>       |
| 4    | Viết queries Logs Insights để lọc và phân tích logs; lưu lại query hữu ích.                                                                      | 08/14/2025   | 08/14/2025      | <https://000008.awsstudygroup.com/>       |
| 5    | Tạo CloudWatch alarm (ví dụ CPU > 70% trong 5 phút) và gắn action tới SNS topic để nhận email.                                                   | 08/15/2025   | 08/15/2025      | <https://000008.awsstudygroup.com/>       |
| 6    | Tạo dashboard hiển thị các metrics chính (CPU, Network, RequestCount) và chia sẻ/ lưu template.                                                  | 08/16/2025   | 08/16/2025      | <https://000008.awsstudygroup.com/>       |
| 7    | Thử kịch bản: kích hoạt alarm bằng tải giả lập hoặc gửi custom metric; kiểm tra notification; dọn dẹp tài nguyên (xóa alarms/SNS/topic nếu cần). | 08/17/2025   | 08/17/2025      | <https://cloudjourney.awsstudygroup.com/> |

### Kết quả mong đợi

- Có hệ thống giám sát cơ bản: metrics + logs + alarms + dashboard.
- Biết cách dùng Logs Insights để phân tích logs và tạo query hữu ích.
- Thiết lập notification (SNS) để nhận cảnh báo qua email hoặc webhook.
- Lập checklist dọn dẹp (xóa alarms/SNS/topic) để tránh cảnh báo dư thừa.

### Tham chiếu nhanh (CLI ví dụ)

```bash
# Liệt kê metric namespaces
aws cloudwatch list-metrics --namespace AWS/EC2

# Gửi custom metric (ví dụ)
aws cloudwatch put-metric-data --metric-name PageViews --namespace "MyApp/Metrics" --value 1 --dimensions Page=/home

# Tạo alarm (ví dụ theo CPU)
aws cloudwatch put-metric-alarm --alarm-name HighCPUAlarm --metric-name CPUUtilization --namespace AWS/EC2 --statistic Average --period 300 --threshold 70 --comparison-operator GreaterThanThreshold --dimensions Name=InstanceId,Value=i-0123456789abcdef0 --evaluation-periods 2 --alarm-actions arn:aws:sns:us-east-1:123456789012:MyTopic

# Tạo SNS topic và subscribe email (nếu cần)
aws sns create-topic --name MyTopic
aws sns subscribe --topic-arn arn:aws:sns:us-east-1:123456789012:MyTopic --protocol email --notification-endpoint you@example.com

# Logs Insights query example (kết quả top 10 IPs truy cập)
# trong console: fields @timestamp, @message | parse @message "\"GET * HTTP/*" as path | stats count() by client_ip | sort by count() desc | limit 10

# Tạo dashboard đơn giản từ JSON
aws cloudwatch put-dashboard --dashboard-name "OpsOverview" --dashboard-body file://dashboard.json

# Xóa alarm (cleanup)
aws cloudwatch delete-alarms --alarm-names HighCPUAlarm
aws sns delete-topic --topic-arn arn:aws:sns:us-east-1:123456789012:MyTopic
```

### Tài liệu tham khảo

- CloudWatch workshop: <https://000008.awsstudygroup.com/>
- CloudWatch docs: https://docs.aws.amazon.com/cloudwatch/
- CloudJourney overview: <https://cloudjourney.awsstudygroup.com/>





