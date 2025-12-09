---
title: "Bản đề xuất"
date: "2025-12-08"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# FindNest

## Nền Tảng AI AWS Serverless cho Tìm Kiếm Nhà Trọ Thông Minh

### 1. Tóm tắt điều hành

Nền tảng **FindNest** tận dụng **khả năng hiểu biết AI** và **kiến trúc AWS Serverless** để chuyển đổi việc tìm kiếm nhà trọ thành trải nghiệm thông minh, theo ngữ cảnh. Bằng cách kết hợp **Amazon Bedrock** để xử lý ngôn ngữ tự nhiên và **Amazon Location Service** để phân tích không gian, nền tảng cho phép người dùng tìm phòng bằng các truy vấn tự nhiên như "phòng giá rẻ gần Thủ Đức có phòng gym và khu vực an toàn."

Hệ thống (Frontend: React trên Amplify, Backend: AWS Lambda + API Gateway) lưu trữ dữ liệu trong DynamoDB, xử lý xác thực qua Cognito, và làm phong phú danh sách với thông tin ngữ cảnh như mật độ quán ăn, tiện ích gần đó, và chỉ số an toàn. Thông báo và xác thực OTP được xử lý qua Amazon SNS. Toàn bộ nền tảng hoạt động trong AWS Free Tier với chi phí ước tính ~$0.5/tháng.

### 2. Tuyên bố vấn đề

#### Vấn đề hiện tại

Các nền tảng hiện nay chỉ hỗ trợ bộ lọc đơn giản như giá hoặc diện tích và thiếu khả năng hiểu ý định người dùng sâu sắc. Người dùng phải tự xem xét hàng trăm tin đăng mà không có sự hỗ trợ AI hoặc thông tin vị trí thông minh. Không có cơ chế nào để hiểu các sở thích phức tạp như _"khu vực an toàn với nhiều lựa chọn ăn uống"_ hoặc _"dễ đi làm về Quận 1."_

#### Giải pháp

Một **nền tảng serverless tăng cường AI** hiểu ý định người dùng thông qua ngôn ngữ tự nhiên, tự động làm phong phú danh sách với dữ liệu ngữ cảnh (nhà hàng, an toàn, tuyến đường), và gợi ý kết quả liên quan bằng **Amazon Bedrock** và **Amazon Location Service**. Backend lọc danh sách lưu trong DynamoDB và xếp hạng kết quả bằng điểm số AI.

#### Lợi ích và hoàn vốn đầu tư

- **Tìm kiếm ngữ nghĩa AI**: Bedrock hiểu ý định người dùng vượt xa bộ lọc thông thường.
- **Gợi ý theo ngữ cảnh**: Danh sách được làm phong phú bởi Location Service mang lại sự liên quan thực tế.
- **Khả năng mở rộng Serverless**: Các dịch vụ được quản lý hoàn toàn tự động mở rộng mà không cần bảo trì.
- **Hiệu quả chi phí**: Tất cả thành phần chạy trong AWS Free Tier cho giai đoạn MVP (~$0.5/tháng).

### 3. Kiến trúc giải pháp

Nền tảng sử dụng thiết kế AWS Serverless module với làm phong phú AI và dữ liệu ngữ cảnh động.

| Thành phần            | Dịch vụ / Công nghệ           |
| --------------------- | ----------------------------- |
| Frontend Hosting      | AWS Amplify (React SPA)       |
| API Backend           | AWS Lambda + API Gateway      |
| Database              | DynamoDB                      |
| File Storage          | S3                            |
| User Management       | Cognito                       |
| Notifications         | Amazon SNS                    |
| Map & Location        | Amazon Location Service       |
| Recommendation Engine | Amazon Bedrock + Lambda Logic |

![Kiến Trúc FindNest](/images/2-Proposal/AWSProject.drawio.png)

### Dịch vụ AWS sử dụng

- **AWS Lambda**: Thực thi logic backend bao gồm xử lý AI và truy vấn tìm kiếm.
- **Amazon API Gateway**: Cung cấp các endpoint RESTful cho yêu cầu từ client.
- **Amazon DynamoDB**: Lưu trữ hồ sơ người dùng, danh sách và dữ liệu ngữ cảnh được làm phong phú.
- **Amazon S3**: Lưu trữ hình ảnh phòng và các tệp tĩnh frontend.
- **AWS Amplify**: Lưu trữ và quản lý triển khai frontend.
- **Amazon Cognito**: Quản lý luồng xác thực và phân quyền.
- **Amazon SNS**: Gửi mã OTP và thông báo người dùng.
- **Amazon Location Service**: Lấy POI xung quanh, tuyến đường và ngữ cảnh an toàn.
- **Amazon Bedrock**: Hiểu tìm kiếm ngôn ngữ tự nhiên và thực hiện xếp hạng ngữ nghĩa.

### Thiết kế thành phần

- **Ứng dụng Frontend**: React SPA được lưu trữ trên Amplify cho trải nghiệm người dùng động và responsive.
- **Lớp API**: Ứng dụng Express triển khai trên Lambda qua API Gateway xử lý tìm kiếm AI, danh sách và làm phong phú.
- **Cơ sở dữ liệu**: Các bảng DynamoDB cho danh sách, người dùng và lịch sử tìm kiếm.
- **Lưu trữ**: S3 bucket lưu trữ hình ảnh; đọc công khai qua signed URLs.
- **Công cụ gợi ý**: Bedrock hiểu truy vấn người dùng và xếp hạng danh sách.
- **Tích hợp bản đồ**: Amazon Location Service cung cấp vị trí ngữ cảnh và trực quan hóa POI.
- **Hệ thống thông báo**: SNS cung cấp OTP và cảnh báo cho danh sách mới.
- **Quản lý người dùng**: Cognito xử lý đăng ký, đăng nhập và token bảo mật.

### 4. Triển khai kỹ thuật

#### Phương pháp Recommendation Engine

- **Giai đoạn MVP**: Bedrock hiểu truy vấn người dùng → Lọc DynamoDB + Làm phong phú Location Service.
- **Giai đoạn mở rộng**: Công việc làm phong phú liên tục để tính toán các chỉ số ngữ cảnh (food_density, safety_score, comfort_index).
- **Giai đoạn nâng cao**: Học thích ứng — lưu phản hồi người dùng để tinh chỉnh phản hồi prompt Bedrock.

#### Yêu cầu kỹ thuật

- **Frontend**: React + Amplify UI với thanh tìm kiếm tích hợp AI và bản đồ vị trí.
- **Backend**: Ứng dụng Lambda Node.js sử dụng AWS SDK cho Bedrock, DynamoDB và Location.
- **Cơ sở dữ liệu**: Các bảng DynamoDB cho người dùng, danh sách và lịch sử tìm kiếm.
- **Lưu trữ**: S3 buckets cho lưu trữ tệp.
- **Xác thực**: Luồng đăng nhập Cognito + SNS OTP.

### 5. Lộ trình & Mốc triển khai

**Lộ trình dự án**

- **Tuần 1-2**: Thiết kế kiến trúc AWS, cấu hình Amplify và triển khai API cơ bản.
- **Tuần 3-4**: Triển khai tìm kiếm ngữ nghĩa Bedrock và logic làm phong phú Location.
- **Tuần 5**: Tích hợp frontend và tinh chỉnh luồng tìm kiếm điều khiển AI.
- **Tuần 6**: Hoàn thiện kiểm thử và triển khai.
- **Sau triển khai**: Thu thập dữ liệu người dùng để cải thiện AI và vòng phản hồi.

### 6. Ước tính ngân sách

### Chi phí hạ tầng

| Thành phần           | Dịch vụ               | Ước tính          |
| -------------------- | --------------------- | ----------------- |
| Lambda + API Gateway | Backend               | $0.22/tháng       |
| DynamoDB             | Database              | $0.10/tháng       |
| S3                   | Storage               | $0.20/tháng       |
| Cognito + SNS        | Authentication + OTP  | $0.13/tháng       |
| Bedrock              | AI Processing         | $7.5/tháng        |
| Location Service     | Map & Geospatial Data | $3.30/tháng       |
| **Tổng cộng**        |                       | **~$24.32/tháng** |

**Lưu ý**: Tất cả dịch vụ hoạt động trong giới hạn sử dụng Free Tier trong giai đoạn MVP với chi phí vận hành tối thiểu.

### 7. Đánh giá rủi ro

#### Ma trận rủi ro

- **Bedrock hiểu sai**: Ảnh hưởng trung bình, xác suất trung bình.
- **Lambda tăng đột biến chi phí (Scaling)**: Ảnh hưởng thấp, xác suất trung bình.
- **Dữ liệu ngữ cảnh không đầy đủ**: Ảnh hưởng trung bình, xác suất thấp.

#### Chiến lược giảm thiểu

- **Kỹ thuật Prompt**: Tối ưu hóa đầu vào Bedrock và dự phòng về bộ lọc đơn giản.
- **Caching**: Cache kết quả Location và AI cho các truy vấn thường xuyên.

#### Kế hoạch dự phòng

- **Hạn chế Bedrock**: Dự phòng về logic lọc chỉ DynamoDB.
- **Vấn đề Timeout**: Chia công việc làm phong phú thành các batch Lambda nhỏ hơn.
- **Tải API cao**: Mở rộng API Gateway với throttling sử dụng.

### 8. Kết quả kỳ vọng

#### Cải tiến kỹ thuật

- Tìm kiếm ngôn ngữ tự nhiên được hỗ trợ AI qua Bedrock.
- Làm phong phú ngữ cảnh tự động bằng Location Service.
- Hạ tầng có khả năng mở rộng, chi phí thấp được hỗ trợ bởi AWS Serverless stack.

#### Giá trị dài hạn

- **Học liên tục**: Cải thiện prompt Bedrock với phản hồi người dùng.
- **Nhận thức ngữ cảnh thông minh**: Xây dựng hồ sơ động cho khu vực và thói quen người dùng.
- **Nền tảng có khả năng mở rộng**: Sẵn sàng tích hợp với Amazon Personalize hoặc tinh chỉnh Bedrock.
- **Hiệu quả chi phí**: Hoàn toàn serverless với bảo trì tối thiểu và không có máy chủ cố định.

---

