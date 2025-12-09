---
title: "Blog 2"
date: "2025-11-25"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# AWS Transform công bố khả năng hiện đại hóa Windows toàn bộ ngăn xếp

**Tác giả:** Prasad Rao | **Ngày:** 01/12/2025

Đầu năm nay vào tháng 5, chúng tôi đã công bố tính sẵn có chung của [AWS Transform cho .NET](https://aws.amazon.com/transform/net), dịch vụ AI đại lý đầu tiên để hiện đại hóa các ứng dụng .NET ở quy mô lớn. Trong giai đoạn áp dụng sớm của dịch vụ, chúng tôi nhận được phản hồi có giá trị cho thấy rằng, ngoài việc hiện đại hóa ứng dụng .NET, bạn muốn hiện đại hóa SQL Server và các framework UI kế thừa. Ứng dụng của bạn thường tuân theo kiến trúc ba tầng—tầng trình bày, tầng ứng dụng và tầng cơ sở dữ liệu—và bạn cần một giải pháp toàn diện có thể chuyển đổi tất cả các tầng này theo cách phối hợp.

Hôm nay, dựa trên phản hồi của bạn, chúng tôi vui mừng thông báo [AWS Transform cho hiện đại hóa Windows toàn bộ ngăn xếp](https://aws.amazon.com/transform/windows), để giảm tải công việc hiện đại hóa phức tạp, tẻ nhạt trên ngăn xếp ứng dụng Windows. Giờ đây bạn có thể xác định các phụ thuộc ứng dụng và cơ sở dữ liệu và hiện đại hóa chúng theo cách được điều phối thông qua trải nghiệm tập trung.

AWS Transform tăng tốc hiện đại hóa Windows toàn bộ ngăn xếp lên đến năm lần trên các tầng ứng dụng, UI, cơ sở dữ liệu và triển khai. Cùng với việc chuyển đổi các ứng dụng .NET Framework sang .NET đa nền tảng, nó di chuyển cơ sở dữ liệu SQL Server sang [Amazon Aurora PostgreSQL-Compatible Edition](https://aws.amazon.com/rds/aurora/features/) với chuyển đổi thủ tục lưu trữ thông minh và tái cấu trúc mã ứng dụng phụ thuộc. Để xác thực và kiểm thử, AWS Transform triển khai ứng dụng lên [Amazon Elastic Compute Cloud (Amazon EC2)](https://aws.amazon.com/ec2) Linux hoặc [Amazon Elastic Container Service (Amazon ECS)](https://aws.amazon.com/ecs/), và cung cấp các mẫu [AWS CloudFormation](https://aws.amazon.com/cloudformation/) và cấu hình triển khai có thể tùy chỉnh cho môi trường sản xuất. AWS Transform cũng đã thêm khả năng hiện đại hóa UI ASP.NET Web Forms sang Blazor.

Có nhiều điều để khám phá, vì vậy trong bài viết này tôi sẽ cung cấp cái nhìn đầu tiên về các khả năng hiện đại hóa Windows toàn bộ ngăn xếp của AWS Transform trên tất cả các tầng.

## Tạo công việc chuyển đổi hiện đại hóa Windows toàn bộ ngăn xếp

AWS Transform kết nối với các kho mã nguồn và máy chủ cơ sở dữ liệu của bạn, phân tích các phụ thuộc ứng dụng và cơ sở dữ liệu, tạo các làn sóng hiện đại hóa và điều phối các chuyển đổi toàn bộ ngăn xếp cho từng làn sóng.

Để bắt đầu với AWS Transform, trước tiên tôi hoàn thành các bước giới thiệu được nêu trong [hướng dẫn bắt đầu với AWS Transform](https://docs.aws.amazon.com/transform/latest/userguide/getting-started.html). Sau khi giới thiệu, tôi đăng nhập vào bảng điều khiển AWS Transform bằng thông tin đăng nhập của mình và tạo một công việc cho hiện đại hóa Windows toàn bộ ngăn xếp.

![Tạo công việc mới cho hiện đại hóa Windows](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/29/1.-createjob-1-2.png)

![Tạo công việc mới bằng cách chọn hiện đại hóa cơ sở dữ liệu SQL Server](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/29/1.-createjob-2-2.png)

Sau khi tạo công việc, tôi hoàn thành các [điều kiện tiên quyết](https://docs.aws.amazon.com/transform/latest/userguide/win-full-stack/sql-server-setup.html). Sau đó, tôi cấu hình [trình kết nối cơ sở dữ liệu](https://docs.aws.amazon.com/transform/latest/userguide/win-full-stack/sql-server-create-job.html) để AWS Transform truy cập an toàn vào cơ sở dữ liệu SQL Server chạy trên Amazon EC2 và [Amazon Relational Database Service (Amazon RDS)](https://aws.amazon.com/rds/). Trình kết nối có thể kết nối với nhiều cơ sở dữ liệu trong cùng một phiên bản SQL Server.

![Tạo trình kết nối cơ sở dữ liệu mới](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/28/2.-DB-Connector.png)

Tiếp theo, tôi [thiết lập trình kết nối](https://docs.aws.amazon.com/transform/latest/userguide/dotnet-creating-repo-connector.html) để kết nối với các kho mã nguồn của tôi.

![Thêm trình kết nối mã nguồn](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/28/3.-source-code-connector.png)

Hơn nữa, tôi có tùy chọn chọn xem tôi có muốn AWS Transform triển khai các ứng dụng đã chuyển đổi hay không. Tôi chọn **Yes** và cung cấp ID tài khoản AWS đích và Vùng AWS để triển khai ứng dụng. Tùy chọn triển khai cũng có thể được cấu hình sau.

![Chọn xem bạn có muốn triển khai ứng dụng đã chuyển đổi](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/28/4.-deploy-apps.png)

Sau khi các trình kết nối được thiết lập, AWS Transform kết nối với các tài nguyên và chạy xác thực để xác minh vai trò IAM, cài đặt mạng và các tài nguyên AWS liên quan.

Sau khi xác thực thành công, AWS Transform phát hiện cơ sở dữ liệu và các kho mã nguồn liên quan của chúng. Nó xác định các phụ thuộc giữa cơ sở dữ liệu và ứng dụng để tạo các làn sóng để chuyển đổi các thành phần liên quan cùng nhau. Dựa trên phân tích này, AWS Transform tạo kế hoạch chuyển đổi dựa trên làn sóng.

![Bắt đầu đánh giá cho cơ sở dữ liệu và kho mã nguồn được phát hiện](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/28/5.-start-assessment.png)

## Đánh giá cơ sở dữ liệu và ứng dụng phụ thuộc

Để đánh giá, tôi xem xét các cơ sở dữ liệu và kho mã nguồn được AWS Transform phát hiện và chọn các nhánh phù hợp cho kho mã. AWS Transform quét các cơ sở dữ liệu và kho mã nguồn này, sau đó trình bày danh sách các cơ sở dữ liệu cùng với các ứng dụng .NET phụ thuộc của chúng và độ phức tạp chuyển đổi.

![Bắt đầu lập kế hoạch làn sóng của các cơ sở dữ liệu và kho đã đánh giá](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/28/6.-start-wave-planning.png)

Tôi chọn các cơ sở dữ liệu và kho đích để hiện đại hóa. AWS Transform phân tích các lựa chọn này và tạo ra **Báo cáo đánh giá hiện đại hóa SQL** toàn diện với kế hoạch làn sóng chi tiết. Tôi tải xuống báo cáo để xem xét kế hoạch hiện đại hóa được đề xuất. Báo cáo bao gồm bản tóm tắt điều hành, kế hoạch làn sóng, các phụ thuộc giữa cơ sở dữ liệu và kho mã, và phân tích độ phức tạp.

![Xem Báo cáo đánh giá hiện đại hóa SQL](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/28/7.-SQL-Summary-report.png)

## Chuyển đổi làn sóng ở quy mô lớn

Kế hoạch làn sóng được tạo bởi AWS Transform bao gồm bốn bước cho mỗi làn sóng. Đầu tiên, nó chuyển đổi lược đồ SQL Server sang PostgreSQL. Thứ hai, nó di chuyển dữ liệu. Thứ ba, nó chuyển đổi mã ứng dụng .NET phụ thuộc để làm cho nó tương thích với PostgreSQL. Cuối cùng, nó triển khai ứng dụng để kiểm thử.

Trước khi chuyển đổi lược đồ SQL Server, tôi có thể tạo một cơ sở dữ liệu PostgreSQL mới hoặc chọn một cơ sở dữ liệu hiện có làm cơ sở dữ liệu đích.

![Chọn hoặc tạo cơ sở dữ liệu đích](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/28/8.-choose-DB-1.png)

Sau khi tôi chọn cơ sở dữ liệu nguồn và đích, AWS Transform tạo báo cáo chuyển đổi để tôi xem xét. AWS Transform chuyển đổi lược đồ SQL Server sang cấu trúc tương thích PostgreSQL, bao gồm bảng, chỉ mục, ràng buộc và thủ tục lưu trữ.

![Tải xuống báo cáo chuyển đổi lược đồ](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/28/9.-download-conversion-report.png)

Đối với bất kỳ lược đồ nào mà AWS Transform không thể tự động chuyển đổi, tôi có thể xử lý chúng thủ công trong bảng điều khiển [AWS Database Migration Service (AWS DMS)](https://aws.amazon.com/dms/). Ngoài ra, tôi có thể sửa chúng trong trình soạn thảo SQL ưa thích của mình và cập nhật phiên bản cơ sở dữ liệu đích.

Sau khi hoàn thành chuyển đổi lược đồ, tôi có tùy chọn tiến hành di chuyển dữ liệu, đây là một bước tùy chọn. AWS Transform sử dụng AWS DMS để di chuyển dữ liệu từ phiên bản SQL Server của tôi sang phiên bản cơ sở dữ liệu PostgreSQL. Tôi có thể chọn thực hiện di chuyển dữ liệu sau, sau khi hoàn thành tất cả các chuyển đổi, hoặc làm việc với dữ liệu kiểm thử bằng cách tải nó vào cơ sở dữ liệu đích của tôi.

![Chọn xem bạn có muốn di chuyển dữ liệu](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/28/10.-migrate-data.png)

Bước tiếp theo là chuyển đổi mã. Tôi chỉ định một nhánh đích cho AWS Transform để tải lên các hiện vật mã đã chuyển đổi. AWS Transform cập nhật codebase để làm cho ứng dụng tương thích với cơ sở dữ liệu PostgreSQL đã chuyển đổi.

![Chỉ định đích nhánh đích cho codebase đã chuyển đổi](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/28/11.-target-branch.png)

Với bản phát hành này, AWS Transform cho hiện đại hóa Windows toàn bộ ngăn xếp chỉ hỗ trợ codebase trong .NET 6 trở lên. Đối với codebase trong .NET Framework 3.1+, trước tiên tôi sử dụng AWS Transform cho .NET để chuyển đổi chúng sang .NET đa nền tảng. Tôi sẽ mở rộng điều này trong phần sau.

Sau khi quá trình chuyển đổi hoàn tất, tôi có thể xem các nhánh nguồn và đích cùng với trạng thái chuyển đổi mã của chúng. Tôi cũng có thể tải xuống và xem xét báo cáo chuyển đổi.

![Tải xuống báo cáo chuyển đổi](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/28/12-download-code-report.png)

## Hiện đại hóa ứng dụng .NET Framework với tầng UI

Một tính năng chính mà chúng tôi phát hành hôm nay là hiện đại hóa các framework UI từ ASP.NET Web Forms sang Blazor. Điều này được bổ sung vào hỗ trợ hiện có để hiện đại hóa các chế độ xem Razor model-view-controller (MVC) sang các chế độ xem Razor ASP.NET Core.

Như đã đề cập trước đó, nếu tôi có một ứng dụng .NET trong .NET Framework kế thừa, thì tôi tiếp tục sử dụng [AWS Transform cho .NET để chuyển đổi nó sang .NET đa nền tảng](https://aws.amazon.com/blogs/aws/aws-transform-for-net-the-first-agentic-ai-service-for-modernizing-net-applications-at-scale/). Đối với các ứng dụng kế thừa với UI được xây dựng trên ASP.NET Web Forms, giờ đây AWS Transform hiện đại hóa tầng UI sang Blazor cùng với việc chuyển đổi mã backend.

AWS Transform cho .NET chuyển đổi các dự án ASP.NET Web Forms sang Blazor trên ASP.NET Core, tạo điều kiện cho việc di chuyển các trang web ASP.NET sang Linux. Tính năng hiện đại hóa UI được bật theo mặc định trong AWS Transform cho .NET trên cả bảng điều khiển web AWS Transform và tiện ích mở rộng Visual Studio.

Trong quá trình hiện đại hóa, AWS Transform xử lý việc chuyển đổi các trang ASPX, các điều khiển tùy chỉnh ASCX và các tệp code-behind, triển khai chúng dưới dạng các thành phần Blazor phía máy chủ thay vì web assembly. Các thay đổi dự án và tệp sau được thực hiện trong quá trình chuyển đổi:

| **Từ**         | **Sang**         | **Mô tả**                                                              |
| -------------- | ---------------- | ---------------------------------------------------------------------- |
| *.aspx, *.ascx | *.razor          | Các trang .aspx và điều khiển tùy chỉnh .ascx trở thành các tệp .razor |
| Web.config     | appsettings.json | Cài đặt Web.config trở thành cài đặt appsettings.json                  |
| Global.asax    | Program.cs       | Mã Global.asax trở thành mã Program.cs                                 |
| *.master       | *layout.razor    | Các tệp Master trở thành các tệp layout.razor                          |

![Hình ảnh minh họa cách các tệp dự án cụ thể được chuyển đổi](https://d2908q01vomqb2.cloudfront.net/da4b9237bacccdf19c0760cab7aec4a8359010b0/2025/11/24/web-forms-to-blazor-project-changes.png)

## Các tính năng mới khác trong AWS Transform cho .NET

Cùng với việc chuyển đổi UI, AWS Transform cho .NET đã thêm hỗ trợ cho nhiều khả năng chuyển đổi hơn và nâng cao trải nghiệm nhà phát triển. Các tính năng mới này bao gồm:

* **Chuyển đổi sang .NET 10 và .NET Standard** – AWS Transform hiện hỗ trợ chuyển đổi sang .NET 10, bản phát hành Hỗ trợ dài hạn (LTS) mới nhất, được phát hành vào ngày 11 tháng 11 năm 2025. Nó cũng hỗ trợ chuyển đổi các thư viện lớp sang .NET Standard, một đặc tả chính thức cho một tập hợp các API phổ biến trên tất cả các triển khai .NET. Hơn nữa, AWS Transform hiện có sẵn với AWS Toolkit cho Visual Studio 2026.
* **Báo cáo chuyển đổi có thể chỉnh sửa** – Sau khi hoàn thành đánh giá, giờ đây bạn có thể xem và tùy chỉnh kế hoạch chuyển đổi dựa trên yêu cầu và sở thích cụ thể của bạn. Ví dụ, bạn có thể cập nhật chi tiết thay thế gói.
* **Cập nhật chuyển đổi theo thời gian thực với thời gian còn lại ước tính** – Tùy thuộc vào kích thước và độ phức tạp của codebase, AWS Transform có thể mất một chút thời gian để hoàn thành việc chuyển đổi. Giờ đây bạn có thể theo dõi các cập nhật chuyển đổi theo thời gian thực cùng với thời gian còn lại ước tính.
* **Markdown các bước tiếp theo** – Sau khi hoàn thành chuyển đổi, AWS Transform hiện tạo một tệp markdown các bước tiếp theo với các nhiệm vụ còn lại để hoàn thành việc chuyển đổi. Bạn có thể sử dụng điều này làm kế hoạch được sửa đổi để lặp lại quá trình chuyển đổi với AWS Transform hoặc sử dụng các trợ lý mã AI để hoàn thành việc chuyển đổi.

## Những điều cần biết

Một số điều cần biết thêm là:

* **Vùng AWS** – AWS Transform cho hiện đại hóa Windows toàn bộ ngăn xếp hiện đã có sẵn chung hôm nay tại [Vùng](https://docs.aws.amazon.com/glossary/latest/reference/glos-chap.html#region) US East (N. Virginia). Để biết tính khả dụng theo vùng và lộ trình tương lai, hãy truy cập [Khả năng AWS theo Vùng](https://builder.aws.com/capabilities/).
* **Giá cả** – Hiện tại, [không có phí bổ sung](https://aws.amazon.com/transform/pricing/) cho các tính năng hiện đại hóa Windows của AWS Transform. Bất kỳ tài nguyên nào bạn tạo hoặc tiếp tục sử dụng trong tài khoản AWS của bạn bằng cách sử dụng đầu ra của AWS Transform sẽ được tính phí theo giá tiêu chuẩn của chúng. Để biết giới hạn và hạn ngạch, hãy tham khảo [Hướng dẫn sử dụng AWS Transform](https://docs.aws.amazon.com/transform/latest/userguide/transform-limits.html).
* **Các phiên bản SQL Server được hỗ trợ** – AWS Transform hỗ trợ chuyển đổi các phiên bản SQL Server từ 2008 R2 đến 2022, bao gồm tất cả các phiên bản (Express, Standard và Enterprise). SQL Server phải được lưu trữ trên Amazon RDS hoặc Amazon EC2 trong cùng Vùng với AWS Transform.
* **Các phiên bản Entity Framework được hỗ trợ** – AWS Transform hỗ trợ hiện đại hóa các phiên bản Entity Framework 6.3 đến 6.5 và Entity Framework Core 1.0 đến 8.0.
* **Bắt đầu** – Để bắt đầu, hãy truy cập [Hướng dẫn sử dụng](https://docs.aws.amazon.com/transform/latest/userguide/win-full-stack/windows-full-stack.html) AWS Transform cho hiện đại hóa Windows toàn bộ ngăn xếp.

— [Prasad](https://www.linkedin.com/in/kprasadrao/)

---

### Về tác giả

**Prasad Rao** là Principal Partner Solutions Architect tại AWS có trụ sở tại Vương quốc Anh. Lĩnh vực tập trung của anh ấy là Hiện đại hóa ứng dụng .NET và Khối lượng công việc Windows trên AWS. Anh ấy tận dụng kinh nghiệm của mình để giúp đỡ các Đối tác AWS trên khắp EMEA cho việc kích hoạt kỹ thuật dài hạn của họ để xây dựng kiến trúc có thể mở rộng trên AWS. Anh ấy cũng hướng dẫn những người đa dạng mới làm quen với đám mây và muốn bắt đầu trên AWS.