---
title: "Kết quả & Outputs"
date: "`r Sys.Date()`"
weight: 4
chapter: false
pre: " <b> 5.3.4. </b> "
---

Sau khi triển khai thành công, terminal sẽ hiển thị phần **Outputs** màu xanh. Đây là các tham số quan trọng để kết nối Frontend với Backend.

#### Stack Outputs

⚠️ **VUI LÒNG SAO CHÉP VÀ LƯU CÁC GIÁ TRỊ NÀY:**

```plaintext
✅  BackendStack

Outputs:
BackendStack.ApiUrl = https://xyz123.execute-api.us-east-1.amazonaws.com/prod/
BackendStack.UserPoolId = us-east-1_AbCdEfGh
BackendStack.UserPoolClientId = 1a2b3c4d5e6f7g8h9i0j
BackendStack.IdentityPoolId = us-east-1:12345678-abcd-1234-abcd-1234567890ab
BackendStack.ImagesBucket = findnest-images-123456789012
BackendStack.MapName = FindNestMap-123456789012
BackendStack.PlaceIndexName = FindNestPlacesV3-123456789012
BackendStack.RouteCalculatorName = FindNestRoutesV3-123456789012
BackendStack.Region = us-east-1

✅  MonitoringStack

Outputs:
MonitoringStack.AlertTopicArn = arn:aws:sns:us-east-1:123456789012:BoardingHouseAlerts
MonitoringStack.DashboardUrl = https://us-east-1.console.aws.amazon.com/cloudwatch/home?region=us-east-1#dashboards:name=SmartBoardingHouse-Monitoring
```

![Deployment Outputs](/images/5-Workshop/5.3-DeployBackend/outputs.png)

#### Giải thích các tham số Output

| Tham số                 | Mô tả                                 | Sử dụng                                             |
| ----------------------- | ------------------------------------- | --------------------------------------------------- |
| **ApiUrl**              | Endpoint cho Backend API              | Frontend sử dụng để gọi API                         |
| **UserPoolId**          | Định danh Cognito User Pool           | Dùng cho xác thực người dùng (Đăng nhập/Đăng ký)    |
| **UserPoolClientId**    | Định danh Cognito App Client          | Dùng cho xác thực người dùng (Đăng nhập/Đăng ký)    |
| **IdentityPoolId**      | Định danh Cognito Identity Pool       | Dùng để hiển thị bản đồ trên Frontend               |
| **ImagesBucket**        | Tên S3 bucket cho hình ảnh            | Dùng để lưu trữ và truy xuất hình ảnh phòng         |
| **MapName**             | Tên bản đồ Location Service           | Dùng để hiển thị bản đồ trên ứng dụng Frontend      |
| **PlaceIndexName**      | Place index của Location Service      | Dùng cho chức năng geocoding và tìm kiếm            |
| **RouteCalculatorName** | Route calculator của Location Service | Dùng để tính toán lộ trình giữa các địa điểm        |
| **Region**              | AWS region                            | Region nơi các tài nguyên được triển khai           |
| **AlertTopicArn**       | SNS Topic ARN cho cảnh báo            | Đăng ký topic này để nhận cảnh báo hệ thống         |
| **DashboardUrl**        | CloudWatch Dashboard URL              | Truy cập dashboard giám sát để xem metrics hệ thống |

#### Các bước tiếp theo

Lưu các outputs này ở một nơi an toàn. Bạn sẽ cần chúng để cấu hình ứng dụng Frontend trong phần tiếp theo.

{{% notice tip %}}
Bạn cũng có thể lấy lại các outputs này sau bằng cách chạy `cdk deploy` lại hoặc kiểm tra outputs của CloudFormation stack trong AWS Console.
{{% /notice %}}

#### Chúc mừng

Bạn đã triển khai thành công hệ thống **Backend Serverless** hiện đại, tích hợp với:

- ✅ AI (Amazon Bedrock với Claude 3)
- ✅ Bản đồ số (Amazon Location Service)
- ✅ Quyền bảo mật chặt chẽ (IAM)
- ✅ Cơ sở dữ liệu có khả năng mở rộng (DynamoDB)
- ✅ Xác thực người dùng (Cognito)
- ✅ RESTful API (API Gateway + Lambda)
- ✅ Giám sát toàn diện (CloudWatch Dashboard & Alarms)
- ✅ Cảnh báo thời gian thực (SNS Notifications)
