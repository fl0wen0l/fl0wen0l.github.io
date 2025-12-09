---
title: "Chạy Script"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: " <b> 5.4.3. </b> "
---

Bây giờ, thực thi script. Script này tương tác, nên nó sẽ hỏi bạn các thông tin chi tiết.

#### Thực thi script

```bash
node seed-admin.js
```

#### Quy trình tương tác

Script sẽ yêu cầu bạn nhập các thông tin sau:

1. **Username**: Nhập username (ví dụ: `superadmin`)
2. **Password**: Nhập mật khẩu mạnh (tối thiểu 8 ký tự, chữ hoa, chữ thường, số, ký tự đặc biệt)
3. **Email**: Nhập địa chỉ email hợp lệ
4. **Full Name**: Nhập tên hiển thị
5. **Confirm**: Gõ `yes` để tiếp tục

#### Kết quả mong đợi

Nếu mọi thứ được cấu hình đúng, bạn sẽ thấy output tương tự như sau:

```plaintext
✅ Environment variables loaded:
   Region: us-east-1
   User Pool ID: us-east-1_xxxxxx
   User Profiles Table: UserProfiles

📝 Creating admin user in Cognito...
   ✅ User created with ID: xxxx-xxxx-xxxx
🔑 Setting permanent password...
   ✅ Password set
👑 Adding user to Admins group...
   ✅ Added to Admins group
📝 Creating admin profile in DynamoDB...
   ✅ Profile created

═══════════════════════════════════════════════
✅ ADMIN USER CREATED SUCCESSFULLY!
═══════════════════════════════════════════════
```

{{% notice success %}}
**Lưu thông tin đăng nhập!** Bạn sẽ cần username và password để đăng nhập vào hệ thống.
{{% /notice %}}

{{% notice warning %}}
Nếu gặp lỗi:

- Kiểm tra file `.env` có giá trị đúng không
- Xác minh AWS credentials đã được cấu hình (`aws configure`)
- Đảm bảo bạn có đủ quyền IAM cần thiết
- Chắc chắn CDK stack đã được triển khai thành công

{{% /notice %}}

#### Script làm gì

Script thực hiện các thao tác sau:

1. ✅ **Tạo user trong Cognito User Pool** với username và password được cung cấp
2. ✅ **Đặt mật khẩu là vĩnh viễn** (không cần thay đổi ở lần đăng nhập đầu tiên)
3. ✅ **Thêm user vào nhóm "Admins"** để có quyền nâng cao
4. ✅ **Tạo bản ghi profile trong DynamoDB** với thông tin người dùng
