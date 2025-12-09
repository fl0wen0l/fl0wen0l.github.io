---
title: "Xác minh xóa tài nguyên"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: " <b> 5.6.2. </b> "
---

Mặc dù chúng ta đã cấu hình `RemovalPolicy.DESTROY` và `autoDeleteObjects: true` trong code CDK, việc kiểm tra kỹ AWS Console để đảm bảo mọi thứ đã bị xóa là một **Best Practice**.

#### A. Kiểm tra Amazon S3

1. Vào [S3 Console](https://console.aws.amazon.com/s3/)
2. Tìm kiếm `findnest` trong danh sách bucket
3. Xác minh bucket `findnest-images-...` đã bị xóa

Nếu bucket vẫn tồn tại:

1. Chọn bucket
2. Click nút **Empty**
3. Xác nhận bằng cách gõ `permanently delete`
4. Sau khi làm trống, click nút **Delete**
5. Xác nhận bằng cách gõ tên bucket

{{% notice info %}}
Cài đặt `autoDeleteObjects: true` trong code thường tự động xử lý điều này bằng cách triển khai một Lambda function tùy chỉnh để xóa bucket trước khi xóa.
{{% /notice %}}

#### B. Kiểm tra DynamoDB Tables

1. Vào [DynamoDB Console](https://console.aws.amazon.com/dynamodb/)
2. Click **Tables** ở sidebar bên trái
3. Xác minh tất cả bảng đã bị xóa:
   - BoardingHouseListings
   - UserProfiles
   - OTPVerifications
   - UserFavorites
   - SupportRequests
   - SearchHistory
   - UserPreferences

Nếu có bảng nào còn lại, chọn chúng và click **Delete**.

#### C. Kiểm tra CloudWatch Logs

1. Vào [CloudWatch Console](https://console.aws.amazon.com/cloudwatch/)
2. Điều hướng đến **Logs** → **Log groups**
3. Tìm kiếm `/aws/lambda/FindNestApi`

Nếu log group vẫn tồn tại (đôi khi logs được tạo trong quá trình xóa được giữ lại):

1. Chọn log group
2. Chọn **Actions** → **Delete log group(s)**
3. Xác nhận xóa

{{% notice tip %}}
Log groups có thể tích lũy phí nhỏ theo thời gian. Nên xóa chúng nếu không còn cần thiết.
{{% /notice %}}

#### D. Kiểm tra Lambda Functions

1. Vào [Lambda Console](https://console.aws.amazon.com/lambda/)
2. Xác minh function `FindNestApi` đã bị xóa

Nếu vẫn tồn tại, chọn nó và click **Actions** → **Delete**.

#### E. Kiểm tra API Gateway

1. Vào [API Gateway Console](https://console.aws.amazon.com/apigateway/)
2. Xác minh `FindNestAPI` đã bị xóa

Nếu vẫn tồn tại, chọn nó và click **Actions** → **Delete API**.

#### F. Kiểm tra Cognito

1. Vào [Cognito Console](https://console.aws.amazon.com/cognito/)
2. Kiểm tra **User Pools** - Xác minh `FindNestUsers` đã bị xóa
3. Kiểm tra **Identity Pools** - Xác minh `FindNestMapAccess` đã bị xóa

#### G. Kiểm tra Location Service

1. Vào [Amazon Location Service Console](https://console.aws.amazon.com/location/)
2. Xác minh các tài nguyên sau đã bị xóa:
   - Map: `FindNestMap`
   - Place Index: `FindNestPlaces`
   - Route Calculator: `FindNestRoutes`

#### H. Kiểm tra CloudFormation

1. Vào [CloudFormation Console](https://console.aws.amazon.com/cloudformation/)
2. Xác minh `BackendStack` hiển thị trạng thái **DELETE_COMPLETE** hoặc không còn trong danh sách

{{% notice success %}}
Nếu bạn không thấy stack hoặc nó hiển thị DELETE_COMPLETE, việc cleanup của bạn đã thành công!
{{% /notice %}}

#### Danh sách kiểm tra xác minh

Xác nhận tất cả tài nguyên đã bị xóa:

- ✅ S3 Bucket (`findnest-images-*`)
- ✅ DynamoDB Tables (tất cả 7 bảng)
- ✅ Lambda Function (`FindNestApi`)
- ✅ API Gateway (`FindNestAPI`)
- ✅ Cognito User Pool (`FindNestUsers`)
- ✅ Cognito Identity Pool (`FindNestMapAccess`)
- ✅ Location Service resources (Map, Place Index, Route Calculator)
- ✅ CloudWatch Log Groups (`/aws/lambda/FindNestApi`)
- ✅ CloudFormation Stack (`BackendStack`)
- ✅ IAM Roles (tự động xóa cùng stack)

{{% notice warning %}}
Nếu có tài nguyên nào còn lại sau 10 phút, xóa thủ công chúng để tránh phí bất ngờ.
{{% /notice %}}
