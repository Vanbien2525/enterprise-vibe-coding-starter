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
