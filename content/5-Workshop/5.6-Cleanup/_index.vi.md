---
title: "Dọn dẹp tài nguyên"
date: "`r Sys.Date()`"
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

#### Tại sao cleanup lại quan trọng?

Trong môi trường Cloud, bạn trả tiền cho những gì bạn cung cấp. Mặc dù các dịch vụ Serverless như Lambda và DynamoDB (On-Demand) có Free Tier hào phóng hoặc chi phí nhàn rỗi thấp, các tài nguyên khác có thể phát sinh phí theo thời gian:

- **S3 Storage**: Bạn trả tiền cho dữ liệu lưu trong buckets
- **Amazon Location Service**: Lưu trữ Place Indexes hoặc sử dụng Maps
- **CloudWatch Logs**: Dữ liệu log được lưu trữ

Để tránh hóa đơn bất ngờ, luôn dọn dẹp môi trường sau khi hoàn thành workshop.

#### Nội dung

- [Hủy Stack](5.6.1-destroy-stack/)
- [Xác minh xóa tài nguyên](5.6.2-verify-deletion/)
- [Kết luận Workshop](5.6.3-conclusion/)
