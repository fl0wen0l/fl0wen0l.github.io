---
title: "Lấy cấu hình"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 5.5.1. </b> "
---

Đảm bảo bạn có **ApiUrl** từ outputs của Phần 5.3 (Deploy Backend).

#### Xác định API URL

Sau khi chạy `cdk deploy`, bạn đã nhận được outputs tương tự:

```plaintext
✅  BackendStack

Outputs:
BackendStack.ApiUrl = https://xyz123.execute-api.us-east-1.amazonaws.com/prod/
BackendStack.UserPoolId = us-east-1_AbCdEfGh
...
```

#### Định dạng API URL

API URL tuân theo định dạng:

```
https://<api-id>.execute-api.<region>.amazonaws.com/prod/
```

**Ví dụ:**

```
https://xyz123.execute-api.us-east-1.amazonaws.com/prod/
```

{{% notice info %}}
`/prod/` ở cuối biểu thị deployment stage. Đây là nơi API Gateway định tuyến tất cả các request đến.
{{% /notice %}}

#### Lưu API URL

Sao chép và lưu URL này. Bạn sẽ sử dụng nó làm base URL cho tất cả các API requests trong các test case tiếp theo.

{{% notice tip %}}
Nếu bạn mất outputs, bạn có thể lấy lại bằng cách:

- Chạy lại `cdk deploy` (nó sẽ không triển khai lại nếu không có gì thay đổi)
- Kiểm tra outputs của CloudFormation stack trong AWS Console
- Vào API Gateway Console và tìm invoke URL của API
  {{% /notice %}}

#### Xác minh API Gateway

Bạn có thể nhanh chóng xác minh API Gateway có thể truy cập bằng cách mở URL trong trình duyệt:

```
https://xyz123.execute-api.us-east-1.amazonaws.com/prod/
```

Bạn sẽ thấy response cho biết API đang chạy (có thể là 404 hoặc welcome message, tùy thuộc vào cấu hình root route của bạn).
