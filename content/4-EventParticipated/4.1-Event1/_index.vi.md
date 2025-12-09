---
title: "Event 1"
date: "2025-11-25"
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---


**Cảm nghĩ cá nhân sau khi tham dự — AWS Cloud Mastery Series #1 (AI/ML & Generative AI)**

Em đã tham dự buổi chia sẻ vào ngày **15 November 2025** tại **Bitexco Financial Tower**. Buổi hội thảo có cấu trúc rõ ràng: phần giới thiệu ngắn, phần tổng quan về dịch vụ AI/ML của AWS, và phần chuyên sâu về Generative AI với demo trực tiếp.

Những kiến thức chính tôi tiếp thu được:

- Amazon SageMaker: tôi nắm lại quy trình end‑to‑end — từ chuẩn bị dữ liệu, labeling, huấn luyện, tinh chỉnh hyperparameters đến triển khai model; demo SageMaker Studio cho thấy nhiều thao tác có thể tự động hóa giúp rút ngắn thời gian lặp.
- Generative AI với Amazon Bedrock: so sánh các foundation models (Claude, LLaMA, Titan), hiểu cách chọn model phù hợp với bài toán, và cách tích hợp các mô hình này vào ứng dụng thật.
- Prompt engineering & RAG: học được các kỹ thuật thiết kế prompt (few‑shot, chain‑of‑thought) và kiến trúc RAG để kết hợp knowledge base, giúp nâng cao độ chính xác cho các câu trả lời domain‑specific.
- Bedrock Agents & Guardrails: khái niệm agent giúp orchestrate nhiều bước xử lý, còn guardrails nhắc nhở về các biện pháp lọc/an toàn nội dung khi dùng generative models.

Điều ấn tượng nhất với tôi:

- Demo xây chatbot generative bằng Bedrock cho thấy pipeline thực tế thường gồm nhiều thành phần (retriever, prompt, model, post‑processing), và tất cả cần được giám sát để đảm bảo chất lượng.
- Phần so sánh foundation models rất hữu ích: không có model “tốt nhất”, mà cần cân nhắc độ chính xác, chi phí, độ trễ và khả năng tùy biến.

Ứng dụng và kế hoạch hành động của tôi:

1. Thử làm một prototype RAG để phục vụ knowledge base nội bộ — mục tiêu có thể demo trong 2 tuần.
2. Thực hiện một bài test nhỏ với Bedrock (hoặc model tương đương) để đánh giá chi phí và chất lượng cho một use‑case nội bộ.
3. Thiết lập một checklist guardrails (filtering, rate limiting, logging) trước khi thử nghiệm trên dữ liệu thật.

Kết luận: Buổi học cung cấp cho tôi cả góc nhìn chiến lược và các hướng thực hành cụ thể để bắt đầu áp dụng Generative AI. Tôi cảm thấy tự tin hơn để thực hiện các lab nhỏ và chuẩn bị báo cáo/slide tóm tắt cho nhóm.


