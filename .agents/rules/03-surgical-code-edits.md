---
description: "Surgical Code Editing Engine and Production Code Quality Standards"
trigger: always_on
---

# Surgical Code Editing & Engineering Best Practices

## 1. Cơ Chế Sửa Code Chính Xác (Surgical Diffs vs Whole File Rewrite)
- **Nghiêm cấm ghi đè toàn bộ (No Whole File Rewrites):**
  - Nếu một file đã có sẵn 300 dòng và bạn chỉ cần thêm 1 hàm hoặc sửa 2 dòng logic, TUYỆT ĐỐI KHÔNG dùng `write_to_file` để ghi đè cả 300 dòng đó.
  - BẮT BUỘC sử dụng tool `replace_file_content` để thực hiện surgical patch.
- **Bảo toàn tính nguyên vẹn của mã nguồn (Preserve Integrity):**
  - Giữ nguyên toàn bộ comment gốc, docstring, header bản quyền và format thư viện của người dùng. Không tự ý reformat lại toàn bộ file theo ý thích nếu không được yêu cầu.
- **Định vị độc nhất (Unique Anchor Matching):**
  - Khi chỉ định `TargetContent`, phải lấy kèm 1-2 dòng context phía trên và phía dưới để đảm bảo chuỗi cần thay thế là **Duy Nhất** trong khoảng dòng `[StartLine, EndLine]`.
  - Khớp chính xác 100% từng dấu cách (spaces), tab và ký tự xuống dòng (`\r\n` hoặc `\n`).

## 2. Tiêu Chuẩn Chất Lượng Code (Production-Ready Standards)
- **Strict Typing:**
  - TypeScript: Không sử dụng kiểu `any` biểu diễn dữ liệu tùy tiện. Sử dụng `unknown` nếu cần kiểm tra type, hoặc định nghĩa `interface`/`type` rõ ràng.
  - Python: Luôn kèm type annotations (`typing.Union`, `Optional`, `List`, `Dict`) hoặc `Pydantic models` cho data schema.
- **Defensive Error Handling:**
  - Mọi logic tương tác mạng (HTTP, gRPC), I/O file, hoặc database phải được bọc trong khối `try...catch` có ý nghĩa.
  - Không được "nuốt ngoại lệ" (swallow errors) như: `catch (e) {}` mà không log hoặc rethrow.
- **Clean Architecture & Single Responsibility:**
  - Controller chỉ làm nhiệm vụ nhận request, validate DTO và gọi Service.
  - Service xử lý business logic, không viết trực tiếp SQL hay truy cập DOM.
  - Repositories/DAOs chịu trách nhiệm truy xuất dữ liệu thuần túy.
