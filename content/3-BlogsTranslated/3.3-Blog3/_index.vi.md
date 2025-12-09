---
title: "Blog 3"
date: "2025-11-25"
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# Tự động hóa thông báo email cho các nhóm quản trị làm việc với Amazon SageMaker Catalog

**Tác giả:** Himanshu Sahni, Jitesh Kumar, và Rajiv Upadhyay | **Ngày:** 21/10/2025

[Amazon SageMaker Catalog](https://aws.amazon.com/sagemaker/catalog/) đơn giản hóa việc khám phá, quản trị và cộng tác cho dữ liệu và AI trên Data Lakehouse, các mô hình AI và ứng dụng. Với Amazon SageMaker Catalog, bạn có thể khám phá và truy cập dữ liệu và mô hình được phê duyệt một cách an toàn bằng cách sử dụng tìm kiếm ngữ nghĩa với siêu dữ liệu được tạo bởi AI tạo sinh hoặc có thể chỉ cần hỏi [Amazon Q Developer](https://aws.amazon.com/q/developer/) bằng ngôn ngữ tự nhiên để tìm dữ liệu của họ.

Các khách hàng doanh nghiệp lớn có nhiều lĩnh vực kinh doanh sản xuất và tiêu thụ dữ liệu bằng cách sử dụng SageMaker Data Catalog trung tâm. Nhiều khách hàng có một nhóm quản trị dữ liệu trung tâm chịu trách nhiệm tạo, xuất bản và duy trì các tiêu chuẩn và thực hành tốt nhất về quản trị dữ liệu trên toàn công ty. Khi nền tảng dữ liệu của khách hàng mở rộng quy mô, việc nhóm quản trị trung tâm duy trì các tiêu chuẩn trên tất cả các nhà sản xuất và người tiêu dùng dữ liệu trở nên thách thức. Do đó, nhiều nhóm quản trị cần giám sát hoạt động của người dùng trong Amazon SageMaker Catalog để đảm bảo các tài sản dữ liệu được xuất bản theo các tiêu chuẩn quản trị tổ chức đã được thiết lập và các thực hành tốt nhất. Trong tình huống này, cần có tự động hóa để các nhóm quản trị trung tâm có thể được thông báo khi các sự kiện quan trọng xảy ra trong Amazon SageMaker Catalog.

Trong bài viết này, chúng tôi chỉ cho bạn cách tạo thông báo tùy chỉnh cho các sự kiện xảy ra trong SageMaker Catalog bằng cách sử dụng [Amazon EventBridge](https://aws.amazon.com/eventbridge/), [AWS Lambda](https://aws.amazon.com/lambda/) và [Amazon Simple Notification Service](https://aws.amazon.com/sns/) (Amazon SNS). Bạn có thể mở rộng giải pháp này để tự động tích hợp SageMaker Catalog với các công cụ quy trình làm việc doanh nghiệp nội bộ như ServiceNow và Helix.

## Tổng quan giải pháp

Kiến trúc giải pháp sau đây cho thấy cách SageMaker Catalog tích hợp với các dịch vụ AWS khác như [AWS IAM Identity Center](https://aws.amazon.com/iam/identity-center/), Amazon EventBridge, [Amazon SQS](https://aws.amazon.com/sqs/), AWS Lambda và Amazon SNS để tạo thông báo tự động nhằm ghi lại các sự kiện quan trọng trong danh mục doanh nghiệp.

![Kiến trúc giải pháp](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/12/Screenshot-2025-10-12-at-10.43.50%20AM.png)

1. Người dùng SageMaker Catalog đăng nhập vào [Amazon SageMaker Unified Studio](https://aws.amazon.com/sagemaker/unified-studio/) bằng IAM Identity Center. Đây có thể là nhà khoa học dữ liệu, kỹ sư máy học hoặc nhà phân tích đang tìm kiếm các bộ dữ liệu đã xuất bản trong công ty. AWS IAM Identity Center đảm bảo rằng chỉ những nhân viên được ủy quyền mới có thể truy cập các tài sản đã được phân loại và tài nguyên ML.
2. Người dùng thực hiện một hoạt động trong SageMaker Catalog. Ví dụ: người dùng tạo một dự án mới hoặc người dùng tìm kiếm một tài sản dữ liệu và tạo yêu cầu đăng ký để truy cập tài sản.
3. Các sự kiện người dùng từ SageMaker Catalog được ghi lại trong Amazon EventBridge. Amazon EventBridge là một dịch vụ bus sự kiện serverless, được quản lý hoàn toàn, được thiết kế để giúp bạn xây dựng các ứng dụng hướng sự kiện có thể mở rộng trên AWS, SaaS và các ứng dụng tùy chỉnh. Amazon EventBridge cung cấp khả năng lọc các sự kiện và cho phép người dùng thực hiện hành động trên các sự kiện cụ thể. Ví dụ về mẫu sự kiện sau trong EventBridge lọc các sự kiện tạo dự án DataZone.

   ```json
   {
     "source": [
       "aws.datazone"
     ],
     "detail": {
       "eventSource": [
         "datazone.amazonaws.com"
       ],
       "eventName": [
         "CreateProject"
       ]
     }
   }
   ```

4. Amazon EventBridge gửi các sự kiện đã lọc đến Amazon SQS. Định tuyến các sự kiện đến hàng đợi SQS cải thiện độ tin cậy và độ bền. Amazon SQS hoạt động như một bộ đệm giữa Amazon EventBridge và AWS Lambda, tách rời các nhà sản xuất sự kiện khỏi người tiêu dùng. Điều này cho phép các hàm Lambda của bạn xử lý tin nhắn theo tốc độ của riêng chúng, ngăn ngừa quá tải trong các đợt lưu lượng tăng đột biến hoặc khi các tài nguyên downstream tạm thời chậm hoặc không khả dụng. Amazon SQS cung cấp lưu trữ bền vững, liên tục cho các sự kiện. Nếu dịch vụ Lambda không khả dụng hoặc bị điều tiết, các tin nhắn vẫn còn trong hàng đợi cho đến khi chúng có thể được xử lý thành công, giảm nguy cơ mất dữ liệu. Có một Dead Letter Queue (DLQ) được đính kèm vào hàng đợi SQS chính. Đính kèm DLQ vào SQS đảm bảo rằng bất kỳ tin nhắn nào không thể xử lý sau nhiều lần thử đều được ghi lại một cách an toàn để kiểm tra và khắc phục sự cố, ngăn chúng chặn hoặc lưu thông vô tận trong hàng đợi chính.
5. Hàm AWS Lambda đọc các tin nhắn từ hàng đợi SQS. Hàm Lambda định dạng thông báo dựa trên nhu cầu của bạn.
6. AWS Lambda xuất bản tin nhắn lên Amazon SNS. Người dùng cuối và Nhóm quản trị trung tâm có thể đăng ký chủ đề SNS để nhận cảnh báo email khi một sự kiện xảy ra trong danh mục SageMaker.
7. Amazon CloudWatch tích hợp với AWS Lambda để giám sát hiệu suất, ghi lại các sự kiện và có thể kích hoạt cảnh báo nếu có bất kỳ sự cố nào xảy ra, đảm bảo quy trình làm việc của bạn chạy trơn tru.

## Điều kiện tiên quyết

Bạn cần thiết lập các tài nguyên điều kiện tiên quyết sau:

* Một tài khoản AWS với [Amazon Virtual Private Cloud](https://aws.amazon.com/vpc/) (Amazon VPC) và mạng cơ sở đã được cấu hình.
* Một miền SageMaker Unified Studio hiện có (làm theo hướng dẫn về [Thiết lập Amazon SageMaker Unified Studio](https://docs.aws.amazon.com/sagemaker-unified-studio/latest/adminguide/setting-up.html)).
* Cấp quyền truy cập Lambda trong SageMaker Unified Studio (bắt buộc để Xuất bản tài sản)
  + Thêm vai trò thực thi Lambda làm vai trò IAM trong SageMaker Unified Studio.
  + Gán vai trò thực thi Lambda cho dự án của bạn trong cổng thông tin SageMaker Unified Studio.

Cấu hình này đảm bảo rằng hàm Lambda có quyền được yêu cầu để truy cập các tài nguyên Data Zone và xuất bản thành công các tài sản từ các dự án SageMaker Unified Studio của bạn.

## Triển khai mã

Xem lại hướng dẫn trên [kho GitHub](https://github.com/aws-samples/sample-sagemaker-unified-studio-governance-notifications) của chúng tôi để triển khai framework trong tài khoản AWS của bạn bằng AWS CDK. CDK cung cấp một kiến trúc thông báo hướng sự kiện cho Amazon SageMaker Unified Studio, tập trung vào các sự kiện tạo dự án và xuất bản tài sản.

**Tài nguyên AWS cốt lõi được triển khai** – Sau đây là các tài nguyên AWS cốt lõi được triển khai:

1. **Quy tắc EventBridge**
   * DataZoneCreateProjectRule: Ghi lại các sự kiện tạo dự án DataZone (`CreateProject`).
   * DataZonePublishAssetRule: Ghi lại các sự kiện xuất bản tài sản DataZone (`CreateListingChangeSet` với hành động `PUBLISH` cho loại thực thể `ASSET`).

2. **Hàng đợi SQS**
   * DataZoneEventQueue: Đệm các sự kiện DataZone từ EventBridge trước khi xử lý.
   * Chính sách hàng đợi: Cho phép EventBridge gửi tin nhắn đến hàng đợi SQS.

3. **Hàm Lambda**
   * ProjectNotificationLambda: Xử lý tin nhắn từ hàng đợi SQS, truy xuất chi tiết sự kiện từ DataZone và gửi thông báo đến chủ đề SNS.
     + Vai trò IAM: Cấp quyền truy cập vào các dịch vụ SQS, SNS, CloudWatch Logs và DataZone.
     + Ánh xạ nguồn sự kiện: Kích hoạt hàm Lambda cho từng tin nhắn SQS.

4. **Chủ đề SNS**
   * LambdaSNSTopic: Nhận thông báo từ hàm Lambda.
     + Đăng ký email: Hai điểm cuối email được đăng ký để nhận thông báo.
   * Thêm ID email của bạn vào chủ đề SNS. Bạn sẽ nhận được một email yêu cầu đăng ký, nhấp vào 'Confirm Subscription'

5. **Quyền**
   * Amazon EventBridge gửi sự kiện đến SQS (yêu cầu quyền SQS), Lambda đọc tin nhắn từ Amazon SQS (yêu cầu vai trò Lambda trong quyền SQS), và Lambda xuất bản lên Amazon SNS (yêu cầu quyền SNS).
   * Chính sách IAM: Vai trò thực thi Lambda có các quyền cần thiết cho SQS, SNS, ghi nhật ký và các hoạt động Data Zone.

## Đầu ra được cung cấp (Đầu ra CloudFormation)

* ARN chủ đề Amazon SNS: Để xuất bản thông báo.
* ARN hàng đợi Amazon SQS: Để đệm sự kiện.
* ARN hàm AWS Lambda: Để xử lý sự kiện.
* ARN quy tắc Amazon EventBridge: Cho cả sự kiện xuất bản tài sản và tạo dự án.

## Thông báo tạo dự án

Thực hiện các bước sau để đăng nhập vào SageMaker Unified Studio và tạo một dự án.

1. Đăng nhập vào Bảng điều khiển SageMaker Unified Studio. Điều này đưa bạn đến màn hình đăng nhập miền Amazon SageMaker Unified Studio (các tùy chọn đăng nhập SSO và IAM).

   ![Đăng nhập SageMaker Unified Studio](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-2.jpg)

2. Chọn **Create Project** trên trang đăng nhập SageMaker Unified Studio.

   ![Tạo dự án](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-3.jpg)

3. Chọn tên dự án theo lựa chọn của bạn, chẳng hạn như 'My_Demo_Project'. Trong Hồ sơ dự án, chọn 'All-Capabilities'.

   ![Dự án Demo](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-4.jpg)

4. Chọn **Continue**. Giữ mọi thứ như mặc định.

   ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-5.jpg)

5. Chọn **Continue**. Trên trang tiếp theo, tạo trên 'Create project'.

   ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-6.jpg)

6. Màn hình cuối cùng của việc tạo dự án

   ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-7.jpg)

7. Thông báo email. Sau khi tạo dự án thành công, bạn sẽ thấy một thông báo email được gửi bởi tự động hóa được triển khai ở trên.

   ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-8.jpg)

## Thông báo xuất bản tài sản

Để xuất bản một tài sản mẫu trong SageMaker Unified Studio.

1. **Quyền Lambda**

   Sau khi CDK Stack tạo vai trò thực thi Lambda 'DatazoneStack-LambdaExecutionRole', sử dụng quy trình sau để tích hợp vai trò này vào dự án SageMaker Studio của bạn. Tích hợp này cho phép các hàm Lambda tương tác với API DataZone trong dự án SageMaker Unified Studio.

   1. Đăng nhập vào SageMaker Unified Studio bằng SSO, nhấp vào Members, Add members.
   2. Tìm vai trò 'DatazoneStack-LambdaExecutionRole' và thêm với vai trò 'Contributor'

      ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-9.png)

      ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-10.png)

      LambdaExecutionRole (<cf-stack-name>-LambdaExecutionRole) đã được thêm làm thành viên vào một dự án trong SageMaker Unified Studio.

2. **Tạo tài sản**
   1. Trong dự án 'My_Demo_Project' của bạn, nhấp vào Data. Chọn dấu cộng để thêm một bộ dữ liệu.

      ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-11.jpg)

   2. Tải lên tệp CSV của bạn bằng cách sử dụng mẫu 'Product_v6.csv' được tìm thấy trong thư mục checkout của kho GitHub 'sample-sagemaker-unified-studio-governance-notifications'.

      ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-12.jpg)

   3. Sử dụng loại bảng là S3/external table.

      ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-13.jpg)

   4. Xem xét và xác nhận rằng các tên cột/thuộc tính trong tệp CSV đã tải lên.

      ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-14.jpg)

   5. Kiểm tra cơ sở dữ liệu Glue (**glue_db_<unique_id>**) để xác nhận rằng bảng đã được tạo và nhập đúng cách

3. **Xuất bản tài sản**
   1. Chọn tài sản, chọn Actions và **Publish to Catalog**.

      ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-15.jpg)

      ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-16.jpg)

   2. Xem tài sản đã xuất bản bên dưới.

      ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-17.jpg)

   3. Trong phần Assets của Project Catalog, xác định mục được đánh dấu và xác minh tên bảng đã xuất bản

      ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-18.jpg)

   4. Chọn tên tài sản để hiển thị các chi tiết và thuộc tính bổ sung về bảng/tài sản.

4. **Cảnh báo email**
   1. Sau khi tài sản được xuất bản lên SageMaker Unified Studio, bạn sẽ nhận được một cảnh báo email được gửi với các chi tiết của tài sản đã xuất bản. Các nhóm quản trị trung tâm có thể sử dụng cảnh báo này để xem xét tài sản đã xuất bản để đảm bảo nó phù hợp với các tiêu chuẩn doanh nghiệp.

      ![](https://d2908q01vomqb2.cloudfront.net/b6692ea5df920cad691c20319a6fffd7a4a766b8/2025/10/09/BDB-4667-image-19.jpg)

      Cảnh báo email được gửi để thông báo cho người dùng khi tài sản đã được xuất bản

## Dọn dẹp

Để dọn dẹp tài nguyên của bạn, hoàn thành các bước sau:

```bash
cdk destroy --profile <PIPELINE-PROFILE>
```

## Kết luận

Trong bài viết này, bạn đã học cách xây dựng hệ thống thông báo tự động cho Amazon SageMaker Unified Studio bằng các dịch vụ AWS. Cụ thể, chúng tôi đã đề cập:

* Cách thiết lập thông báo hướng sự kiện từ Amazon SageMaker Unified Studio tận dụng Amazon EventBridge, AWS Lambda và Amazon SNS
* Quy trình từng bước triển khai giải pháp bằng AWS CDK
* Các ví dụ thực tế về giám sát các sự kiện quan trọng như tạo dự án và xuất bản tài sản
* Cách tích hợp quyền AWS Lambda với SageMaker Unified Studio cho các hoạt động an toàn
* Các thực hành tốt nhất để triển khai kiểm soát quản trị thông qua thông báo tự động

Amazon SageMaker Catalog giúp các nhóm quản trị luôn được thông báo về các hoạt động danh mục trong thời gian thực, cho phép họ duy trì các tiêu chuẩn tổ chức khi các nền tảng Dữ liệu và ML của họ mở rộng quy mô. Kiến trúc linh hoạt và có thể được mở rộng để tích hợp với các công cụ quy trình làm việc doanh nghiệp như ServiceNow hoặc để giám sát các loại sự kiện bổ sung dựa trên nhu cầu của tổ chức của bạn.

Chúng tôi mong được nghe cách bạn điều chỉnh giải pháp này cho nhu cầu quản trị của tổ chức mình. Fork mã CDK từ kho của chúng tôi và chia sẻ trải nghiệm triển khai của bạn trong phần bình luận bên dưới.

---

## Về các tác giả

### Himanshu Sahni

[Himanshu](https://www.linkedin.com/in/himanshu-sahni-68b3281b/) là Senior Data and AI Architect trong AWS Professional Services. Himanshu chuyên xây dựng các giải pháp Dữ liệu và Phân tích cho khách hàng doanh nghiệp bằng cách sử dụng các công cụ và dịch vụ AWS. Anh ấy là chuyên gia về các công cụ AI/ML và Big Data như Spark, AWS Glue và Amazon EMR. Ngoài công việc, Himanshu thích chơi cờ vua và quần vợt.

### Rajiv Upadhyay

[Rajiv](https://www.linkedin.com/in/rajiv-upadhyay1/) là Data Architect tại AWS, chuyên xây dựng các giải pháp Dữ liệu và Phân tích cho khách hàng doanh nghiệp bằng cách sử dụng các công cụ và dịch vụ AWS. Anh ấy hướng dẫn các tổ chức trong hành trình chuyển đổi số của họ, với chuyên môn về data lake, quản trị dữ liệu và các giải pháp AI/ML.

### Jitesh Kumar

[Jitesh](https://www.linkedin.com/in/jitesh-kumar-23b510a/) là Senior Customer Solutions Manager tại Amazon Web Services (AWS), nơi anh ấy giúp các tổ chức nhận ra toàn bộ tiềm năng của các công nghệ đám mây. Đam mê thúc đẩy đổi mới kỹ thuật số, Jitesh kết hợp kiến thức kỹ thuật sâu với tư duy khách hàng là trên hết để hướng dẫn các doanh nghiệp trong hành trình chuyển đổi đám mây của họ và mang lại kết quả kinh doanh có thể đo lường được.