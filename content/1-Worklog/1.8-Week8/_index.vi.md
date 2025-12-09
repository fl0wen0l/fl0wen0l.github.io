---
title: "Worklog Tuần 8"
date: "2025-11-25"
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

## Tổng quan

Tuần 8 tập trung vào Auto Scaling Group (ASG) và Elastic Load Balancing — cách thiết kế giải pháp có khả năng mở rộng tự động dựa trên tải, quản lý launch template/launch configuration, thiết lập health checks và chính sách scale (scale out/scale in). Bạn sẽ thực hành tạo Launch Template, tạo ASG, kết hợp với Application Load Balancer, kiểm thử scale bằng cách tạo tải giả lập và dọn dẹp tài nguyên.

### Yêu cầu trước

- Tài khoản AWS có quyền tạo Launch Template, Auto Scaling Group, và Load Balancer.
- Có kiến thức cơ bản về EC2, VPC, Security Group và AWS CLI.
- Có thể sử dụng công cụ để giả lập tải (stress test) hoặc tạo nhiều request cùng lúc.

### Mục tiêu

- Hiểu khái niệm Launch Template/Launch Configuration, Auto Scaling Group và cách hoạt động của policies.
- Thiết lập ALB (Application Load Balancer) và liên kết ASG với ALB target group.
- Cấu hình health checks và policy scale dựa trên CPU/Request metrics.
- Thử nghiệm scale bằng cách tạo tải và quan sát hành vi scale out/scale in.
- Dọn dẹp tài nguyên sau bài tập.

### Kế hoạch công việc (7 ngày)
| Ngày | Công việc                                                                                                                              | Ngày bắt đầu | Ngày hoàn thành | Tài liệu tham khảo                        |
| ---- | -------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 1    | Nghiên cứu lý thuyết: Launch Template vs Launch Configuration, ASG lifecycle, target groups và health checks.                          | 08/11/2025   | 08/11/2025      | <https://000006.awsstudygroup.com/>       |
| 2    | Tạo Launch Template (AMI, instance type, user data) hoặc Launch Configuration nếu cần.                                                 | 08/12/2025   | 08/12/2025      | <https://000006.awsstudygroup.com/>       |
| 3    | Tạo Application Load Balancer (ALB) và target group; cấu hình listener và security group cho ALB.                                      | 08/13/2025   | 08/13/2025      | <https://000006.awsstudygroup.com/>       |
| 4    | Tạo Auto Scaling Group liên kết với Launch Template và target group; cấu hình min/max/desired capacity.                                | 08/14/2025   | 08/14/2025      | <https://000006.awsstudygroup.com/>       |
| 5    | Thiết lập scaling policies (policy theo CPU/ALB RequestCount), bật CloudWatch alarms để trigger scale.                                 | 08/15/2025   | 08/15/2025      | <https://000006.awsstudygroup.com/>       |
| 6    | Thực hiện stress test (ví dụ: `ab`, `siege`, hoặc script curl nhiều luồng) để kích hoạt scale out; theo dõi CloudWatch metrics và log. | 08/16/2025   | 08/16/2025      | <https://000006.awsstudygroup.com/>       |
| 7    | Dọn dẹp: xóa ASG, terminate instances, xóa ALB, xóa Launch Template nếu không cần; ghi lại lessons learned.                            | 08/17/2025   | 08/17/2025      | <https://cloudjourney.awsstudygroup.com/> |

### Kết quả mong đợi

- Thiết lập thành công ASG với Launch Template và ALB, scale tự động hoạt động theo policies.
- Hiểu các chỉ số quan trọng trên CloudWatch và cách dùng chúng để điều chỉnh policy.
- Có quy trình kiểm thử và dọn dẹp rõ ràng để tránh chi phí dư thừa.

### Tham chiếu nhanh (CLI ví dụ)

```bash
# Tạo launch template (ví dụ)
aws ec2 create-launch-template --launch-template-name my-template --version-description "v1" --launch-template-data '{"ImageId":"ami-0123456789abcdef0","InstanceType":"t3.micro","KeyName":"MyKey"}'

# Tạo target group cho ALB
aws elbv2 create-target-group --name my-targets --protocol HTTP --port 80 --vpc-id vpc-01234567

# Tạo ALB (ví dụ)
aws elbv2 create-load-balancer --name my-alb --subnets subnet-aaa subnet-bbb --security-groups sg-01234567

# Tạo Auto Scaling Group
aws autoscaling create-auto-scaling-group --auto-scaling-group-name my-asg --launch-template LaunchTemplateName=my-template,Version=1 --min-size 1 --max-size 4 --desired-capacity 1 --vpc-zone-identifier "subnet-aaa,subnet-bbb" --target-group-arns arn:aws:elasticloadbalancing:...:targetgroup/my-targets/...

# Tạo scale policy theo CPU
aws autoscaling put-scaling-policy --auto-scaling-group-name my-asg --policy-name scale-out-policy --scaling-adjustment 1 --adjustment-type ChangeInCapacity

# Kích hoạt stress test (ví dụ dùng ab)
ab -n 10000 -c 200 http://my-alb-dns-name/

# Cleanup
aws autoscaling delete-auto-scaling-group --auto-scaling-group-name my-asg --force-delete
aws ec2 delete-launch-template --launch-template-name my-template
aws elbv2 delete-load-balancer --load-balancer-arn arn:aws:elasticloadbalancing:...:loadbalancer/app/my-alb/...
```

### Tài liệu tham khảo

- Auto Scaling workshop: <https://000006.awsstudygroup.com/>
- ALB & ASG docs: https://docs.aws.amazon.com/autoscaling/
- CloudJourney overview: <https://cloudjourney.awsstudygroup.com/>





