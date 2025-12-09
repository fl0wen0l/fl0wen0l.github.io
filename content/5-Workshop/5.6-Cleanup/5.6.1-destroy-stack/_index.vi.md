---
title: "Hủy Stack"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: " <b> 5.6.1. </b> "
---

Vẻ đẹp của **Infrastructure as Code (IaC)** với AWS CDK là bạn có thể phá bỏ toàn bộ hệ thống chỉ với một lệnh duy nhất.

CDK biết chính xác những gì nó đã tạo và sẽ xóa tài nguyên theo đúng thứ tự phụ thuộc.

#### Di chuyển đến thư mục CDK

Đảm bảo bạn đang ở thư mục `cdk`:

```bash
cd cdk
```

#### Chạy lệnh Destroy

Thực thi lệnh sau:

```bash
cdk destroy
```

#### Xác nhận

CLI sẽ yêu cầu xác nhận:

```plaintext
Are you sure you want to delete: BackendStack (y/n)?
```

Gõ `y` và nhấn **Enter**.

#### Quá trình xóa

Quá trình xóa thường mất **3-5 phút**.

Bạn sẽ thấy output tương tự:

```plaintext
BackendStack: destroying...

 ✅  BackendStack: destroyed

Stack ARN:
arn:aws:cloudformation:us-east-1:123456789012:stack/BackendStack/...
```

#### Những gì bị xóa

CDK sẽ tự động xóa tất cả tài nguyên theo đúng thứ tự:

1. **API Gateway** - REST API và deployment
2. **Lambda Function** - Bao gồm IAM roles liên quan
3. **DynamoDB Tables** - Tất cả 7 bảng
4. **Cognito User Pool** - Users và groups
5. **Cognito Identity Pool** - Identity mappings
6. **S3 Bucket** - Bao gồm tất cả objects (do `autoDeleteObjects: true`)
7. **Location Service** - Maps, Place Index, Route Calculator
8. **CloudWatch Log Groups** - Lambda execution logs
9. **IAM Roles and Policies** - Tất cả service permissions
10. **CloudFormation Stack** - Stack chính

{{% notice info %}}
Cấu hình `RemovalPolicy.DESTROY` trong code CDK đảm bảo rằng các tài nguyên chứa dữ liệu (như DynamoDB tables và S3 buckets) cũng được xóa.
{{% /notice %}}

#### Theo dõi tiến trình xóa

Bạn có thể theo dõi tiến trình xóa trong:

- **Terminal**: Xem CLI output
- **AWS Console**: Vào CloudFormation → Stacks → BackendStack → Tab Events

#### Khắc phục sự cố xóa

Nếu quá trình xóa thất bại:

**Vấn đề: S3 Bucket không trống**

```bash
# Xóa thủ công bucket
aws s3 rm s3://findnest-images-YOUR-ACCOUNT-ID --recursive

# Sau đó thử destroy lại
cdk destroy
```

**Vấn đề: DynamoDB Table có Deletion Protection**

Nếu bạn đã chỉnh sửa bảng thủ công trong console và bật deletion protection, bạn cần tắt nó trước:

1. Vào DynamoDB Console
2. Chọn bảng
3. Vào tab **Additional settings**
4. Tắt **Deletion protection**
5. Thử lại `cdk destroy`

**Vấn đề: Lambda Execution Role đang được sử dụng**

Đợi vài phút để Lambda executions hoàn thành, sau đó thử lại.

{{% notice warning %}}
Khi bạn chạy `cdk destroy` và xác nhận, quá trình không thể hoàn tác. Tất cả dữ liệu sẽ bị xóa vĩnh viễn.
{{% /notice %}}
