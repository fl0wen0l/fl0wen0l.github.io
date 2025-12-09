---
title: "Worklog Tuần 2"
date: "2025-11-25"
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---


### Mục tiêu tuần 2:



### Kết quả đạt được tuần 2:

* Hiểu AWS là gì và nắm được các nhóm dịch vụ cơ bản: 
  * Compute
  * Storage
  * Networking 
  weight: 2
  * ...

  ## Tổng quan

  Tuần 2 tập trung vào thiết kế, triển khai và bảo mật một môi trường Virtual Private Cloud (VPC) trên AWS. Công việc bao gồm: thiết kế mạng, cấu hình firewall (Security Groups & NACLs), triển khai EC2, cấu hình Site-to-Site VPN, và tự động hoá hạ tầng bằng IaC (CloudFormation/Terraform). Việc dọn dẹp tài nguyên và ghi chép là phần không thể thiếu để đảm bảo có thể tái tạo và tiết kiệm chi phí.

  ### Yêu cầu trước

  - Tài khoản AWS có quyền tạo VPC, EC2, VPN và các tài nguyên liên quan.
  - Đã cài `awscli` và cấu hình cơ bản (Access Key, Secret Key, region mặc định).
  - Kiến thức cơ bản về CloudFormation hoặc Terraform nếu định thực hành IaC.
  - Tham khảo: <https://cloudjourney.awsstudygroup.com/>

  ### Mục tiêu

  - Hiểu các khái niệm cơ bản của VPC và cơ chế bảo mật mạng (Security Groups & NACLs).
  - Thiết kế và triển khai VPC với public/private subnets, bảng định tuyến, Internet Gateway và NAT Gateway.
  - Triển khai và bảo mật EC2 (SSH, EBS, IAM role).
  - Thiết lập Site-to-Site VPN và kiểm tra định tuyến/luồng dữ liệu.
  - Viết template IaC (CloudFormation/Terraform) để tự động hoá việc triển khai.
  - Dọn dẹp tài nguyên và ghi lại các bước để đảm bảo tái tạo.

  ### Kế hoạch công việc (7 ngày)
  | Ngày | Công việc                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                        |
  | ---- | ------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
  | 1    | Nghiên cứu kiến trúc VPC và lên sơ đồ thiết kế (public/private subnets, route tables).                                                      | 08/11/2025   | 08/11/2025      | <https://000003.awsstudygroup.com/>       |
  | 2    | Xây dựng VPC: tạo VPC, subnets (public/private), route tables, Internet Gateway, NAT Gateway; cấu hình Security Groups và NACL cơ bản.      | 08/12/2025   | 08/12/2025      | <https://000003.awsstudygroup.com/>       |
  | 3    | Triển khai EC2: chọn AMI và instance type, khởi tạo EC2 trong subnet phù hợp, cấu hình key pair và IAM role, gắn EBS.                       | 08/13/2025   | 08/13/2025      | <https://000004.awsstudygroup.com/>       |
  | 4    | Tăng cường bảo mật mạng cho EC2: tạo rule cho SSH/HTTP/HTTPS, kiểm tra kết nối, (nếu cần) gán Elastic IP và xác nhận định tuyến ra IGW/NAT. | 08/14/2025   | 08/14/2025      | <https://000004.awsstudygroup.com/>       |
  | 5    | Thiết lập Site-to-Site VPN: tạo Customer Gateway, Virtual Private Gateway và kết nối VPN; kiểm tra route propagation và kết nối end-to-end. | 08/15/2025   | 08/15/2025      | <https://000003.awsstudygroup.com/>       |
  | 6    | Thực hành IaC: viết template CloudFormation hoặc Terraform để provision VPC, subnets, EC2, SG và VPN; thử deploy từ template.               | 08/16/2025   | 08/16/2025      | <https://000037.awsstudygroup.com/>       |
  | 7    | Dọn dẹp: terminate instances, detach & snapshot volume nếu cần, xóa VPC và ghi lại bài học kinh nghiệm cùng tài liệu hướng dẫn.             | 08/17/2025   | 08/17/2025      | <https://cloudjourney.awsstudygroup.com/> |

  ### Kết quả mong đợi

  - Thiết kế và provision VPC an toàn (public & private subnets, IGW, NAT) theo best practice.
  - Thiết lập Security Groups và NACLs để kiểm soát luồng truy cập mạng tới EC2 và subnet.
  - Triển khai EC2 phù hợp với AMI và instance type đã chọn, cấu hình SSH và EBS.
  - Thiết lập và xác minh kết nối Site-to-Site VPN giữa AWS và mạng on-prem (nếu có).
  - Tạo template IaC để tự động hoá việc triển khai và kiểm tra khôi phục.
  - Hoàn thành dọn dẹp và tài liệu hoá các bước để tránh phát sinh chi phí.

  ### Tham chiếu nhanh (ví dụ CLI)

  ```bash
  # Tạo VPC (ví dụ đơn giản)
  aws ec2 create-vpc --cidr-block 10.0.0.0/16

  # Tạo subnet
  aws ec2 create-subnet --vpc-id vpc-xxxx --cidr-block 10.0.1.0/24 --availability-zone us-east-1a

  # Khởi tạo EC2 (ví dụ)
  aws ec2 run-instances --image-id ami-0123456789abcdef0 --count 1 --instance-type t3.micro --key-name MyKey --security-group-ids sg-xxxx --subnet-id subnet-aaa

  # Tạo Customer Gateway (ví dụ)
  aws ec2 create-customer-gateway --type ipsec.1 --public-ip 203.0.113.12 --bgp-asn 65000

  # Xoá (cleanup) ví dụ
  aws ec2 delete-vpc --vpc-id vpc-xxxx
  ```

  ### Tài liệu tham khảo

  - VPC & networking workshop: <https://000003.awsstudygroup.com/>
  - EC2 workshop: <https://000004.awsstudygroup.com/>
  - IaC examples: <https://000037.awsstudygroup.com/>
  - CloudJourney overview: <https://cloudjourney.awsstudygroup.com/>

  ---

  Nếu bạn muốn, tôi có thể:
  - Bổ sung các lệnh CLI chi tiết cho từng ngày (an toàn, không destructive mặc định).
  - Tạo mẫu IaC (Terraform/CloudFormation) tối thiểu để provision VPC + EC2 (bạn cần review trước khi chạy).
  - Dịch sang tiếng Anh/nội dung tóm tắt cho báo cáo.



