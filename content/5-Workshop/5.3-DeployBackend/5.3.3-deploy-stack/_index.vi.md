---
title: "Triển khai Stack"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 5.3.3. </b> "
---

Bây giờ, hãy biến thiết kế này thành hiện thực.

#### Bước 1: Synthesize

Chạy lệnh synthesize để tạo CloudFormation template:

```bash
cdk synth
```

Lệnh này tạo CloudFormation template trong thư mục `cdk.out`. Nếu không có lỗi màu đỏ xuất hiện, bạn có thể tiếp tục.

{{% notice info %}}
Quá trình synthesize chuyển đổi code CDK (TypeScript) thành CloudFormation template (JSON/YAML).
{{% /notice %}}

#### Bước 2: Deploy

Triển khai stack lên AWS:

```bash
cdk deploy --require-approval never
```

Flag `--require-approval never` bỏ qua xác nhận cho các thay đổi IAM.

{{% notice warning %}}
Quá trình triển khai sẽ mất khoảng **5-10 phút** để cung cấp tất cả tài nguyên.
{{% /notice %}}


Trong quá trình triển khai, bạn sẽ thấy:

- Tiến trình tạo CloudFormation stack
- Từng tài nguyên đang được tạo (DynamoDB tables, Lambda functions, API Gateway, v.v.)
- Cập nhật trạng thái cho mỗi tài nguyên

Đợi quá trình triển khai hoàn tất thành công trước khi chuyển sang bước tiếp theo.
