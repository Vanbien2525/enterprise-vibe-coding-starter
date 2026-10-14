---
description: "Quy chuẩn an toàn và hiệu năng cơ sở dữ liệu PostgreSQL"
trigger: model_decision
---

# PostgreSQL Safety & Performance Directives
- **Data Integrity:** Mọi ID bắt buộc dùng `UUIDv7`, tiền tệ lưu `INTEGER/BIGINT`.
- **Atomic Transactions:** Mọi thao tác sửa từ 2 bảng trở lên bắt buộc nằm trong một Transaction.
- **Indexes:** Đánh chỉ mục B-Tree trên các cột khóa ngoại và cột lọc (`status`, `created_at`).
- **Zero Loss:** Tuyệt đối không xóa dữ liệu hàng loạt không có `WHERE`.
