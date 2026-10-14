---
description: "Strict Safety Guardrails, Accidental Data Loss Prevention, and Secret Protection"
trigger: always_on
---

# Global Safety & Disaster Prevention Guardrails

> [!CAUTION]
> Mọi thao tác có khả năng xóa dữ liệu, ghi đè không thể phục hồi hoặc gây gián đoạn hệ thống PHẢI được cảnh báo rõ ràng và chỉ thực thi khi có sự đồng ý rõ ràng từ người dùng.

## 1. Danh Mục Lệnh Cấm Tự Ý Chạy (Forbidden Destructive Commands)
Tuyệt đối KHÔNG tự động sinh và chạy ngầm các lệnh sau:
- **Xóa file/thư mục hàng loạt:**
  - PowerShell: `Remove-Item -Recurse -Force`, `rmdir /s /q`
  - Linux syntax: `rm -rf /`, `rm -rf *`
- **Can thiệp Git phá hủy lịch sử:**
  - `git push --force`, `git push -f`
  - `git reset --hard` (khi đang có uncommitted changes)
  - `git clean -fd` (khi chưa kiểm tra danh sách file untracked)
- **Phá hủy Cơ Sở Dữ Liệu:**
  - SQL: `DROP DATABASE`, `DROP TABLE`, `TRUNCATE TABLE`, `DELETE FROM ...` (không có mệnh đề `WHERE`).
  - Migration CLI: `prisma migrate reset`, `npx drizzle-kit drop`, `alembic downgrade base`.
- **Dịch vụ Đám mây & Hạ tầng:**
  - `gcloud projects delete`, `aws s3 rb --force`, `terraform destroy`.

## 2. Bảo Mật Thông Tin Nhạy Cảm & Secrets (Zero-Leakage Policy)
- **Nghiêm cấm rò rỉ Credentials:**
  - Không bao giờ in trực tiếp giá trị của các file nhạy cảm ra màn hình (`.env`, `.env.production`, `id_rsa`, `*.pem`, `*.key`, `service-account.json`).
  - Khi hướng dẫn người dùng tạo file mới, luôn sử dụng placeholder giả lập: `API_KEY="your_api_key_here"`, `DB_PASSWORD="your_secure_password"`.
- **Kiểm tra .gitignore trước khi tạo file:**
  - Trước khi tạo file cấu hình chứa secret mới, phải đảm bảo file đó đã được khai báo trong `.gitignore`. Nếu chưa có, lập tức thêm vào `.gitignore`.

## 3. Nguyên Tắc Backup Trước Khi Can Thiệp File Trọng Yếu
- Khi sửa đổi các file cấu hình gốc của hệ thống hoặc dự án (`docker-compose.yml`, `nginx.conf`, `tsconfig.json`, `settings.json`), nếu phạm vi thay đổi lớn, phải lưu lại bản backup hoặc xác nhận đã commit Git sạch sẽ trước khi thực hiện.
