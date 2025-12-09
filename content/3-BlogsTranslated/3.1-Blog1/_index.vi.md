---
title: "Blog 1"
date: "2025-12-02"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
# Tăng tốc phát triển AI sử dụng Amazon SageMaker AI với MLflow serverless

**Tác giả:** Donnie Prakoso | **Ngày:** 02/12/2025

Kể từ khi chúng tôi [công bố Amazon SageMaker AI với MLflow vào tháng 6/2024](https://aws.amazon.com/blogs/aws/manage-ml-and-generative-ai-experiments-using-amazon-sagemaker-with-mlflow/), khách hàng của chúng tôi đã sử dụng các máy chủ theo dõi MLflow để quản lý quy trình thử nghiệm [machine learning (ML)](https://aws.amazon.com/ai/machine-learning/) và AI của họ. Dựa trên nền tảng này, chúng tôi tiếp tục phát triển trải nghiệm MLflow để làm cho việc thử nghiệm thậm chí còn dễ tiếp cận hơn.

Hôm nay, tôi rất vui mừng thông báo rằng [Amazon SageMaker AI với MLflow](https://aws.amazon.com/sagemaker/ai/experiments/) hiện bao gồm khả năng serverless loại bỏ việc quản lý cơ sở hạ tầng. Khả năng MLflow mới này biến đổi việc theo dõi thử nghiệm thành trải nghiệm ngay lập tức, theo yêu cầu với khả năng mở rộng tự động loại bỏ nhu cầu lập kế hoạch dung lượng.

Sự chuyển đổi sang quản lý không cơ sở hạ tầng về cơ bản thay đổi cách các nhóm tiếp cận thử nghiệm AI—các ý tưởng có thể được kiểm tra ngay lập tức mà không cần lập kế hoạch cơ sở hạ tầng, cho phép quy trình phát triển lặp đi lặp lại và khám phá nhiều hơn.

## Bắt đầu với Amazon SageMaker AI và MLflow

Hãy để tôi hướng dẫn bạn tạo phiên bản MLflow serverless đầu tiên của mình.

Tôi điều hướng đến [bảng điều khiển Amazon SageMaker AI Studio](https://console.aws.amazon.com/sagemaker) và chọn ứng dụng **MLflow**. Thuật ngữ **MLflow Apps** thay thế thuật ngữ **MLflow tracking servers** trước đây, phản ánh cách tiếp cận đơn giản hóa, tập trung vào ứng dụng.

![](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/25/news-2025-11-sagemaker-mlflow-rev1-2.png)

Ở đây, tôi có thể thấy đã có một MLflow App mặc định được tạo sẵn. Trải nghiệm MLflow đơn giản hóa này giúp tôi bắt đầu thực hiện các thử nghiệm dễ dàng hơn.

Tôi chọn **Create MLflow App** và nhập tên. Ở đây, tôi có cả [vai trò AWS Identity and Access Management (IAM)](https://aws.amazon.com/iam/) và bucket [Amazon Simple Service (Amazon S3)](https://aws.amazon.com/s3/) đã được cấu hình sẵn. Tôi chỉ cần sửa đổi chúng trong **Advanced settings** nếu cần.

![](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/11/news-2025-11-sagemaker-mlflow-1.png)

Đây là nơi cải tiến lớn đầu tiên trở nên rõ ràng—quá trình tạo hoàn tất trong khoảng 2 phút. Sự sẵn có ngay lập tức này cho phép thử nghiệm nhanh chóng mà không cần trì hoãn kế hoạch cơ sở hạ tầng, loại bỏ thời gian chờ đợi trước đây làm gián đoạn quy trình thử nghiệm.

![](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/11/news-2025-11-sagemaker-mlflow-2.png)

Sau khi được tạo, tôi nhận được một MLflow [Amazon Resource Name (ARN)](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference-arns.html) để kết nối từ notebook. Việc quản lý đơn giản hóa có nghĩa là không cần quyết định về kích thước máy chủ hoặc lập kế hoạch dung lượng. Tôi không còn cần chọn giữa các cấu hình khác nhau hoặc quản lý dung lượng cơ sở hạ tầng, điều này có nghĩa là tôi có thể tập trung hoàn toàn vào thử nghiệm. Bạn có thể tìm hiểu cách sử dụng MLflow SDK tại [Tích hợp MLflow với môi trường của bạn trong Hướng dẫn phát triển Amazon SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/mlflow-track-experiments.html).

![](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/15/news-2025-11-sagemaker-mlflow-6.png)

Với sự hỗ trợ MLflow 3.4, giờ đây tôi có thể truy cập các khả năng mới cho phát triển [AI tạo sinh](https://aws.amazon.com/generative-ai/). MLflow Tracing ghi lại các đường thực thi chi tiết, đầu vào, đầu ra và siêu dữ liệu trong suốt vòng đời phát triển, cho phép gỡ lỗi hiệu quả trên các hệ thống AI phân tán.

![](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/11/news-2025-11-sagemaker-mlflow-4.png)

Khả năng mới này cũng giới thiệu quyền truy cập xuyên miền và xuyên tài khoản thông qua chia sẻ [AWS Resource Access Manager (AWS RAM)](https://aws.amazon.com/ram/). Sự cộng tác nâng cao này có nghĩa là các nhóm trên các miền và tài khoản AWS khác nhau có thể chia sẻ các phiên bản MLflow một cách an toàn, phá vỡ các rào cản tổ chức.

![](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/11/news-2025-11-sagemaker-mlflow-5.png)

## Tốt hơn khi kết hợp: Tích hợp Pipelines

[Amazon SageMaker Pipelines](https://aws.amazon.com/sagemaker/ai/pipelines/) được tích hợp với MLflow. SageMaker Pipelines là một dịch vụ điều phối quy trình làm việc serverless được xây dựng đặc biệt cho [tự động hóa machine learning operations (MLOps) và large language model operations (LLMOps)](https://aws.amazon.com/sagemaker/ai/mlops/)—các thực hành triển khai, giám sát và quản lý các mô hình ML và LLM trong sản xuất. Bạn có thể dễ dàng xây dựng, thực thi và giám sát các quy trình làm việc AI từ đầu đến cuối có thể lặp lại với giao diện người dùng kéo thả trực quan hoặc Python SDK.

![](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/23/news-2025-11-sagemaker-mlflow-rev1-0.jpg)

Từ một pipeline, một MLflow App mặc định sẽ được tạo nếu chưa có sẵn. Tên thử nghiệm có thể được xác định và các số liệu, tham số và hiện vật được ghi lại vào MLflow App như đã xác định trong mã của bạn. SageMaker AI với MLflow cũng được tích hợp với các khả năng phát triển mô hình SageMaker AI quen thuộc như [SageMaker AI JumpStart](https://aws.amazon.com/sagemaker/ai/jumpstart/) và [Model Registry](https://docs.aws.amazon.com/sagemaker/latest/dg/model-registry.html), cho phép tự động hóa quy trình làm việc từ đầu đến cuối từ chuẩn bị dữ liệu đến tinh chỉnh mô hình.

## Những điều cần biết

Dưới đây là các điểm quan trọng cần lưu ý:

* **Giá cả** – Khả năng MLflow serverless mới được cung cấp miễn phí. Lưu ý rằng có các giới hạn dịch vụ được áp dụng.
* **Sẵn có** – Khả năng này có sẵn trong các Vùng AWS sau: US East (N. Virginia, Ohio), US West (N.California, Oregon), Asia Pacific (Mumbai, Seoul, Singapore, Sydney, Tokyo), Canada (Central), Europe (Frankfurt, Ireland, London, Paris, Stockholm), South America (São Paulo).
* **Nâng cấp tự động:** Nâng cấp phiên bản MLflow tại chỗ diễn ra tự động, cung cấp quyền truy cập vào các tính năng mới nhất mà không cần công việc di chuyển thủ công hoặc lo ngại về khả năng tương thích. Dịch vụ hiện hỗ trợ MLflow 3.4, cung cấp quyền truy cập vào các khả năng mới nhất bao gồm các tính năng theo dõi nâng cao.
* **Hỗ trợ di chuyển** – Bạn có thể sử dụng công cụ xuất-nhập MLflow mã nguồn mở có sẵn tại [mlflow-export-import](https://github.com/mlflow/mlflow-export-import) để giúp di chuyển từ các Tracking Server hiện có, cho dù chúng từ SageMaker AI, tự lưu trữ, hoặc khác sang MLflow serverless (MLflow Apps).

Bắt đầu với MLflow serverless bằng cách truy cập [Amazon SageMaker AI Studio](https://aws.amazon.com/sagemaker/ai/studio/) và tạo MLflow App đầu tiên của bạn. MLflow serverless cũng được hỗ trợ trong SageMaker Unified Studio để có tính linh hoạt quy trình làm việc bổ sung.

Chúc bạn thử nghiệm vui vẻ!

— [Donnie](https://www.linkedin.com/in/donnieprakoso)

---

### Về tác giả

**Donnie Prakoso** là một kỹ sư phần mềm, người tự xưng là barista, và Principal Developer Advocate tại AWS. Với hơn 17 năm kinh nghiệm trong ngành công nghệ, từ viễn thông, ngân hàng đến startup. Anh ấy hiện đang tập trung vào việc giúp các nhà phát triển hiểu nhiều công nghệ khác nhau để biến ý tưởng của họ thành hiện thực. Anh ấy yêu thích cà phê và bất kỳ cuộc thảo luận nào về bất kỳ chủ đề nào từ microservices đến AI/ML.