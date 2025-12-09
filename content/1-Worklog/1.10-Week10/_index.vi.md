---
title: "Worklog Tuần 10"
date: "2025-11-25"
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

## Tổng quan

Tuần 10 kết hợp hai phần: **Route 53 Resolver** (hệ thống DNS trong AWS cho các môi trường hybrid/on‑prem) và **AWS CLI** (công cụ dòng lệnh để quản lý tài nguyên). Mục tiêu là hiểu cách thiết lập resolver cho giải quyết tên giữa VPC và mạng on‑prem, đồng thời thành thạo một số lệnh AWS CLI thiết yếu để tự động hóa các thao tác quản trị.

### Yêu cầu trước

- Tài khoản AWS có quyền tạo Route 53 Resolver endpoints và thay đổi VPC settings.
- Có quyền cài đặt và cấu hình `awscli` trên máy cục bộ hoặc Cloud9.
- Hiểu cơ bản về DNS, VPC và networking.

### Mục tiêu

- Hiểu chức năng của Route 53 Resolver: inbound/outbound endpoints, rules và rule associations.
- Thiết lập inbound/outbound endpoints để cho phép DNS resolution giữa VPC và on‑prem (hoặc data center khác).
- Thực hành các lệnh AWS CLI cơ bản: cài đặt, cấu hình, liệt kê/tạo/xoá tài nguyên, dùng `--query`/`--output` để xử lý kết quả.
- Viết các lệnh CLI để tự động hóa các bước triển khai và dọn dẹp.

### Kế hoạch công việc (7 ngày)
| Ngày | Công việc                                                                                                                         | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                        |
| ---- | --------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 1    | Nghiên cứu Route 53 Resolver: inbound/outbound endpoint, resolver rules, rule associations.                                       | 08/11/2025   | 08/11/2025      | <https://000010.awsstudygroup.com/>       |
| 2    | Thiết kế sơ đồ DNS cho kịch bản hybrid: xác định forwarders, IP on‑prem, security group cho endpoints.                            | 08/12/2025   | 08/12/2025      | <https://000010.awsstudygroup.com/>       |
| 3    | Tạo Resolver outbound endpoint và inbound endpoint (nếu cần), và cấu hình rule để forward domain nội bộ.                          | 08/13/2025   | 08/13/2025      | <https://000010.awsstudygroup.com/>       |
| 4    | Kiểm tra resolution từ VPC tới on‑prem và ngược lại; kiểm tra logging/debugging nếu DNS không hoạt động.                          | 08/14/2025   | 08/14/2025      | <https://000010.awsstudygroup.com/>       |
| 5    | Cài đặt và cấu hình AWS CLI: `aws configure`, quản lý profiles, sử dụng `--profile` cho nhiều account.                            | 08/15/2025   | 08/15/2025      | <https://000011.awsstudygroup.com/>       |
| 6    | Thực hành lệnh CLI để liệt kê, tạo và xoá tài nguyên (VPC, EC2, S3, Route53 Resolver); sử dụng `--query` và `jq` để parse output. | 08/16/2025   | 08/16/2025      | <https://000011.awsstudygroup.com/>       |
| 7    | Tạo script deploy/cleanup (bash/powershell) để tự động hoá tạo resolver + rule, test và dọn dẹp.                                  | 08/17/2025   | 08/17/2025      | <https://cloudjourney.awsstudygroup.com/> |

### Kết quả mong đợi

- Thiết lập được Route 53 Resolver endpoints và rules phù hợp với sơ đồ mạng hybrid.
- Kiểm chứng resolution giữa VPC và on‑prem hoạt động.
- Thành thạo các lệnh AWS CLI cơ bản để tự động hoá các bước triển khai và dọn dẹp.

### Tham chiếu nhanh (CLI ví dụ)

```bash
# AWS CLI: cài & cấu hình (nếu chưa có)
# Windows PowerShell example to install/verify
pip install --user awscli
aws --version
aws configure

# Tạo outbound resolver endpoint
aws route53resolver create-resolver-endpoint --creator-request-id create-out-1 --name "outbound-endpoint-1" --direction OUTBOUND --security-group-ids sg-01234567 --ip-addresses SubnetId=subnet-aaa,Ip=10.0.1.10 SubnetId=subnet-bbb,Ip=10.0.2.10

# Tạo inbound resolver endpoint (nếu cần)
aws route53resolver create-resolver-endpoint --creator-request-id create-in-1 --name "inbound-endpoint-1" --direction INBOUND --security-group-ids sg-01234567 --ip-addresses SubnetId=subnet-ccc,Ip=10.0.3.10

# Tạo forwarding rule (ví dụ forward domain corp.example to outbound endpoints)
aws route53resolver create-resolver-rule --creator-request-id rule-1 --name "forward-corp" --rule-type FORWARD --domain-name "corp.example." --target-ips Ip=203.0.113.5 --resolver-endpoint-id rslvr-out-0123456789abcdef

# Liệt kê resolver endpoints
aws route53resolver list-resolver-endpoints

# Gán rule vào VPC
aws route53resolver associate-resolver-rule --resolver-rule-id rslvr-rr-012345 --name-association "assoc1" --vpc-id vpc-01234567

# Ví dụ AWS CLI cơ bản: liệt kê instances với query
aws ec2 describe-instances --query 'Reservations[*].Instances[*].[InstanceId,State.Name,PrivateIpAddress]' --output table

# Sử dụng --profile để làm việc với nhiều account
aws s3 ls --profile staging

# Ví dụ cleanup (xóa rule)
aws route53resolver delete-resolver-rule --resolver-rule-id rslvr-rr-012345

```

### Tài liệu tham khảo

- Route53 Resolver workshop: <https://000010.awsstudygroup.com/>
- AWS CLI workshop: <https://000011.awsstudygroup.com/>
- Route53 Resolver docs: https://docs.aws.amazon.com/route53resolver/





