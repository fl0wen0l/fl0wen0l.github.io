---
title: "Tự đánh giá"
date: "2025-11-25"
weight: 6
chapter: false
pre: " <b> 6. </b> "
---


Trong suốt thời gian thực tập tại **[Amazone Web Service - AWS]** từ **[06-09-2025]** đêế **[09-12-2025]**, tôi đã có cơ hội học hỏi, rèn luyện và áp dụng kiến thức đã được trang bị tại trường vào môi trường làm việc thực tế.  
Tôi đã tham gia **[mô tả ngắn gọn dự án hoặc công việc chính]**, qua đó cải thiện kỹ năng **[liệt kê kỹ năng: lập trình, phân tích, viết báo cáo, giao tiếp…]**.  

Về tác phong, tôi luôn cố gắng hoàn thành tốt nhiệm vụ, tuân thủ nội quy, và tích cực trao đổi với đồng nghiệp để nâng cao hiệu quả công việc.
Để phản ánh một cách xây dựng và có tính hành động, tôi đã tổ chức lại nội dung tự đánh giá thành các phần rõ ràng: bối cảnh thực tập, mục tiêu đặt ra, thành tựu chính, kỹ năng phát triển, ví dụ cụ thể, thách thức và cách khắc phục, phản hồi nhận được, và kế hoạch phát triển cá nhân.

**Bối cảnh thực tập**
- Vai trò: [Backend + Security]
- Nhóm / Dự án: [Find Nest]
- Thời gian: **[24-9-2025]** – **[07-12-2025]**

**Mục tiêu tôi đặt ra lúc bắt đầu**
- Học và áp dụng kiến thức nền tảng đám mây (mạng AWS, S3, VPC endpoints).
- Xây dựng và tài liệu hoá một lab truy cập S3 an toàn từ môi trường hybrid sử dụng VPC endpoints.
- Nâng cao kỹ năng tự động hoá (CloudFormation, bash, AWS CLI).
- Cải thiện kỹ năng làm việc nhóm, báo cáo và thuyết trình.

**Thành tựu chính**
- Thiết kế và soạn thảo một workshop đầy đủ: “Truy cập S3 an toàn từ môi trường hybrid bằng VPC Endpoints”, bao gồm sơ đồ kiến trúc, hướng dẫn từng bước và quy trình dọn dẹp.
- Triển khai và xác thực các stack CloudFormation dùng trong workshop, tối giản thời gian thiết lập thủ công cho người tham gia khoảng ~15 phút.
- Tạo các quy trình kiểm thử tái lập được cho cả Gateway và Interface endpoints, đồng thời ghi lại các ghi chú xử lý sự cố DNS và VPN phổ biến.
- Viết tài liệu rõ ràng và dịch các nội dung chính sang cả tiếng Việt và tiếng Anh để phục vụ nhiều đối tượng hơn.

**Kỹ năng phát triển**
- Kỹ thuật: AWS VPC, Route 53 Resolver, mô phỏng Site‑to‑Site VPN (strongSwan), S3 endpoints (Gateway & Interface), CloudFormation, AWS CLI, Session Manager.
- Phần mềm: Script shell để tự động hoá, soạn thảo Markdown và sơ đồ minh hoạ.
- Kỹ năng mềm: Viết kỹ thuật, chuyển giao kiến thức (workshop), hợp tác đa chức năng.

**Ví dụ cụ thể / Bằng chứng**
- Nội dung workshop: thư mục `content/5-Workshop/*` (kiến trúc, bước lab, cleanup).
- Tự động hóa: các template CloudFormation được nhúng trong lab (ví dụ: PLCloudSetup, PLOnpremSetup).
- Kiểm thử thực hiện: tải lên/tải xuống qua Session Manager trên EC2 sử dụng cả Gateway và Interface endpoints.

**Thách thức và cách khắc phục**
- Việc phân giải DNS cho PrivateLink ban đầu không ổn định trên hệ thống DNS mô phỏng on‑prem; tôi đã cấu hình Route 53 resolver với quy tắc chuyển tiếp và ghi lại các bước cụ thể để tái tạo và khắc phục.
- Tải lên file lớn (ví dụ 1GB) đôi khi gặp timeout; tôi bổ sung hướng dẫn sử dụng file mẫu nhỏ hơn khi băng thông hạn chế và thêm mẹo kiểm tra/khởi động lại thao tác tải lên.

**Phản hồi nhận được & hành động đã thực hiện**
- Phản hồi: Yêu cầu làm lab thân thiện hơn với người mới bắt đầu và cung cấp bài kiểm tra nhanh cho người tham gia.
- Hành động: Thêm phần "quick start" (smoke test) thực hiện các kiểm tra tối thiểu và một phần riêng cho xử lý sự cố nâng cao.

**Các lĩnh vực cần phát triển (kế hoạch)**
1. Kỷ luật & quản lý thời gian: áp dụng kế hoạch công việc hàng tuần với các ticket ưu tiên và ghi chú tiến độ ngắn hàng ngày; review định kỳ với người hướng dẫn.
2. Tư duy giải quyết vấn đề: thực hành theo quy trình có cấu trúc (giả thuyết → kiểm tra → cô lập) và lưu kết quả vào cơ sở kiến thức chung.
3. Giao tiếp: thuyết trình một buổi walkthrough lab cho nhóm và yêu cầu phản hồi cụ thể về độ rõ ràng và nhịp độ.

**Mục tiêu trong 6 tháng tới**
- Tham gia đóng góp ít nhất hai module workshop nữa và tự động hoá một bài kiểm thử end‑to‑end với CI (ví dụ: chạy kiểm tra CloudFormation cơ bản).
- Hoàn thành một chứng chỉ AWS hoặc khóa đào tạo nội bộ liên quan đến mạng hoặc bảo mật.

**Suy ngẫm cuối cùng**
Kỳ thực tập này là cầu nối mạnh mẽ giữa học thuật và thực tế. Tôi đã cải thiện cả năng lực kỹ thuật (mạng, dịch vụ AWS, tự động hoá) lẫn kỹ năng nghề nghiệp (tài liệu, hợp tác, tiếp nhận phản hồi). Tôi sẽ tiếp tục hoàn thiện các tài liệu đã tạo và sử dụng phản hồi để làm cho workshop tiếp theo rõ ràng và bền vững hơn cho người tham gia.

### Tóm tắt đánh giá nhanh
| Lĩnh vực              | Điểm (1–5) | Ghi chú                                                              |
| --------------------- | ---------: | -------------------------------------------------------------------- |
| Kiến thức kỹ thuật    |          4 | Hiểu biết vững về VPC endpoints và mẫu Route 53 resolver             |
| Khả năng học hỏi      |          4 | Tiếp thu nhanh các dịch vụ AWS và phương pháp xử lý sự cố            |
| Chủ động              |          5 | Chủ động xây dựng tài liệu workshop và tự động hoá một số bước       |
| Trách nhiệm & tin cậy |          4 | Hoàn thành nhiệm vụ đúng hạn, có cải tiến liên tục                   |
| Giao tiếp             |          3 | Đã tiến bộ nhưng cần luyện tập thuyết trình ngắn gọn                 |
| Giải quyết vấn đề     |          3 | Tốt trong việc xác định nguyên nhân; cần tăng tốc và mở rộng phạm vi |


