---
title: "Triển khai Backend"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

#### Giới thiệu về Infrastructure as Code (IaC)

Thay vì thao tác thủ công trên AWS Console ("ClickOps"), chúng ta sử dụng **AWS CDK** để định nghĩa toàn bộ hạ tầng. Trong file `cdk/lib/backend-stack.ts`, chúng ta đã thiết kế một hệ thống Serverless hoàn chỉnh.

Khi bạn chạy lệnh `cdk deploy`, CDK sẽ biên dịch code này thành **CloudFormation Template**, và AWS tự động cung cấp các tài nguyên tương ứng.

![Kiến trúc](/images/5-Workshop/5.3-DeployBackend/architecture.png)

#### Nội dung

- [Kiến trúc chi tiết](5.3.1-architecture/)
- [Cài đặt Dependencies](5.3.2-install-dependencies/)
- [Triển khai Stack](5.3.3-deploy-stack/)
- [Kết quả & Outputs](5.3.4-results/)
