---
description: "Git Workflow, Branching Strategy, and Conventional Commits"
trigger: always_on
---

# Git Workflow & Conventional Commits Standards

## 1. Conventional Commits Pattern
Mọi commit message được đề xuất phải tuân thủ chuẩn Conventional Commits quốc tế:
`<type>(<scope>): <short description>`

### Danh mục Types được chấp thuận:
- `feat`: Thêm một chức năng mới cho người dùng.
- `fix`: Sửa một lỗi (bug) trong mã nguồn.
- `refactor`: Tái cấu trúc mã nguồn mà không làm thay đổi hành vi bên ngoài hay thêm tính năng.
- `perf`: Cải thiện hiệu năng (performance optimization).
- `test`: Thêm hoặc sửa các bài kiểm thử (unit test, e2e test).
- `chore`: Cập nhật build tools, dependencies, config mà không ảnh hưởng đến runtime logic.
- `docs`: Cập nhật tài liệu (README.md, docstrings).

### Ví dụ Commit Hợp Lệ:
- `feat(auth): implement refresh token rotation with redis`
- `fix(payment): prevent duplicate transaction on network retry`
- `refactor(database): optimize user query using concurrent index`

## 2. Quy Tắc An Toàn Git
- Trước khi commit, luôn chạy `git status` và `git diff --staged` để kiểm tra:
  - Không commit nhầm các file rác: `.DS_Store`, `Thumbs.db`, `.idea/`, `.vscode/`.
  - Không commit file build: `node_modules/`, `dist/`, `build/`, `target/`.
  - Không commit bất kỳ file môi trường nào (`.env*`).

## 3. Chiến Lược Phân Nhánh Trunk-Based (Branching Strategy)
- **`main` là nguồn chân lý duy nhất (Single Source of Truth)**:
  - Tuyệt đối KHÔNG commit hoặc push code trực tiếp lên `main`.
  - Mọi tính năng, sửa lỗi hoặc cải tiến phải được thực hiện trên một nhánh tạm thời (ephemeral feature branch).
- **Quy chuẩn đặt tên nhánh**:
  - Tính năng mới: `feature/<tên-tính-năng>` (ví dụ: `feature/auth-login`, `feature/payment-stripe`)
  - Sửa lỗi: `fix/<tên-lỗi>` (ví dụ: `fix/uuid-generation`, `fix/sql-injection-risk`)
  - Tái cấu trúc: `refactor/<tên-nhiệm-vụ>`

## 4. Quy Trình Pull Request (PR) với GitHub Actions & Google Labs Jules
1. **Mở Pull Request**:
   - Nhánh nguồn (Head): `feature/<tên-tính-năng>`
   - Nhánh đích (Base): `main`
2. **Kiểm tra tự động (CI Pipeline)**:
   - GitHub Actions kích hoạt, khởi chạy container PostgreSQL 16 và chạy bộ test tự động.
   - Jules Bot tự động rà soát an ninh mã nguồn, logic kinh doanh và hiệu năng.
3. **Cơ chế tự sửa lỗi của Jules (Self-Healing)**:
   - Nếu có bài test thất bại hoặc phát hiện lỗi bảo mật, Jules sẽ tự động viết bản sửa lỗi và commit trực tiếp vào nhánh PR đó.
   - CI tự động chạy lại kiểm tra phiên bản mới.
4. **Quyền quyết định thuộc về con người (Human Merge Authority)**:
   - Chỉ khi cả GitHub Actions và Jules đều báo Xanh (Passed ✅), người dùng (Human) mới vào kiểm tra lại và nhấn **Merge pull request** để gộp code vào `main`.

## 5. Dọn Dẹp Sau Khi Merge (Branch Lifecycle & Deletion)
- **Nhánh tạm thời phải bị xóa ngay sau khi merge**:
  - Không giữ lại các nhánh tính năng đã merge để tránh hiện tượng rác nhánh (Branch Rot).
  - Toàn bộ commit và mã nguồn đã được lưu trữ an toàn trong `main`.
- **Quy trình xóa**:
  - Trên GitHub: Nhấn nút **Delete branch** hoặc bật tính năng *"Automatically delete head branches"*.
  - Ở máy cá nhân (Local):
    ```bash
    git checkout main
    git pull origin main
    git branch -d <tên-nhánh-tạm>
    git remote prune origin
    ```

