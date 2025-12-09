---
title: "Workshop"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Xây dựng Serverless Backend với AWS CDK

#### Tổng quan

Trong workshop này, bạn sẽ xây dựng một **Serverless Backend** hoàn chỉnh cho ứng dụng **FindNest** - một nền tảng cho thuê nhà trọ. Bạn sẽ sử dụng **AWS CDK (Cloud Development Kit)** để định nghĩa và triển khai hạ tầng dưới dạng code.

Bạn sẽ học cách:

- Triển khai hạ tầng serverless sử dụng AWS CDK với TypeScript
- Xây dựng RESTful APIs với API Gateway và Lambda
- Triển khai xác thực với Amazon Cognito
- Lưu trữ dữ liệu trong DynamoDB với data modeling phù hợp
- Tích hợp khả năng AI với Amazon Bedrock (Claude 3)
- Thêm chức năng bản đồ với Amazon Location Service
- Áp dụng security best practices với IAM policies
- Quản lý vòng đời hạ tầng (deploy và cleanup)

#### Kiến trúc

Bạn sẽ triển khai kiến trúc serverless hiện đại bao gồm:

- **API Gateway** - RESTful API endpoints
- **AWS Lambda** - Serverless compute (Node.js)
- **Amazon DynamoDB** - NoSQL database (7 bảng)
- **Amazon Cognito** - Xác thực và phân quyền người dùng
- **Amazon S3** - Lưu trữ hình ảnh
- **Amazon Location Service** - Bản đồ và geocoding
- **Amazon Bedrock** - AI/ML với Claude 3
- **Amazon SNS** - Thông báo SMS
- **CloudWatch Logs** - Monitoring và logging

#### Nội dung

1. [Tổng quan Workshop](5.1-Workshop-overview/)
2. [Chuẩn bị](5.2-Prerequiste/)
3. [Triển khai Backend](5.3-DeployBackend/)
4. [Tạo dữ liệu khởi tạo](5.4-SeedingData/)
5. [Kiểm tra hệ thống](5.5-SystemValidation/)
6. [Dọn dẹp tài nguyên](5.6-Cleanup/)
