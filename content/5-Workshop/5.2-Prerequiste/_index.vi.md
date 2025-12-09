---
title: "Các bước chuẩn bị"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

#### Quyền IAM

Để triển khai stack Findnest Backend một cách an toàn, tuân thủ nguyên tắc đặc quyền tối thiểu, hãy thêm chính sách JSON sau vào IAM User của bạn.

Chính sách này tập trung nghiêm ngặt vào các dịch vụ được định nghĩa trong code CDK của bạn (DynamoDB, Cognito, Location, v.v.)

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "FindnestDeploymentPermissions",
      "Effect": "Allow",
      "Action": [
        "cloudformation:*",
        "s3:*",
        "iam:*",
        "lambda:*",
        "apigateway:*",
        "dynamodb:*",
        "cognito-idp:*",
        "cognito-identity:*",
        "geo:*",
        "sns:*",
        "bedrock:*",
        "cloudwatch:*",
        "logs:*",
        "ssm:GetParameter",
        "sts:AssumeRole"
      ],
      "Resource": "*"
    }
  ]
}
```

#### Khởi tạo tài nguyên (CDK Bootstrap)

Trong lab này, chúng ta sẽ sử dụng region N. Virginia (`us-east-1`).
Để chuẩn bị môi trường workshop, chúng ta cần khởi tạo tài nguyên CDK Bootstrap. Quá trình này tạo một S3 Bucket để lưu trữ code Lambda và các template CloudFormation của bạn.

**1. Cài đặt Dependencies**

Đảm bảo bạn đã cài đặt các công cụ cốt lõi trên máy local của mình:

```bash
npm install -g aws-cdk
```

**2. Bootstrap Môi trường**

Chạy lệnh sau trong terminal của bạn để triển khai bootstrap stack. Chấp nhận tất cả các giá trị mặc định.

```bash
cdk bootstrap aws://<YOUR-ACCOUNT-ID>/us-east-1
```

(Thay thế `<YOUR-ACCOUNT-ID>` bằng AWS Account ID 12 chữ số của bạn)

Đợi cho stack CloudFormation `CDKToolkit` đạt trạng thái `CREATE_COMPLETE`.
1 S3 Bucket đã được tạo để lưu trữ assets.
Môi trường hiện đã sẵn sàng cho triển khai serverless.
