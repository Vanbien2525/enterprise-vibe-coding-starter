---
description: "Windows OS Architecture and Native PowerShell Execution Standards"
trigger: always_on
---

# Windows Operating System & PowerShell Runtime Directives

Hiện tại Agent đang chạy trên môi trường **Windows 10/11**, Shell thực thi mặc định là **PowerShell**.
Tuyệt đối không sinh ra các cú pháp của Linux Bash gây lỗi runtime.

## 1. Bảng Chuyển Đổi Cú Pháp (Linux Bash -> Windows PowerShell)

| Thao tác | CẤM DÙNG (Linux / Bash) | BẮT BUỘC DÙNG (Windows PowerShell) |
| :--- | :--- | :--- |
| **Set biến môi trường** | `export PORT=3000` | `$env:PORT = "3000"` |
| **Đọc nội dung file** | `cat file.txt` | `Get-Content -Path "file.txt" -Raw` |
| **Nối nhiều câu lệnh** | `cd app && npm start` | `Set-Location app; npm start` (Dùng `;` thay vì `&&`) |
| **Tìm kiếm chuỗi text** | `grep "error" app.log` | `Select-String -Path "app.log" -Pattern "error"` |
| **Tạo thư mục** | `mkdir -p a/b/c` | `New-Item -ItemType Directory -Path "a\b\c" -Force` |
| **Kiểm tra tồn tại** | `test -f file.txt` | `Test-Path -Path "file.txt"` |
| **Xóa file an toàn** | `rm file.txt` | `Remove-Item -Path "file.txt" -Force` |
| **Gọi REST API** | `curl -X POST ...` | `Invoke-RestMethod` hoặc `curl.exe -X POST ...` |

## 2. Quy Tắc Đường Dẫn File (File Paths on Windows)
- Luôn sử dụng dấu gạch chéo ngược `\` cho Windows path hoặc bọc đường dẫn trong ngoặc kép nếu có khoảng trắng: `"C:\Users\Admin\My Project\file.ts"`.
- Tuyệt đối không giả định tồn tại các thư mục gốc của Unix như: `/tmp`, `/var`, `/home`, `/etc`.
- Khi cần tạo thư mục tạm, luôn dùng thư mục scratch được chỉ định hoặc biến `$env:TEMP`.

## 3. Quản Lý Process & Port Trên Windows
- **Tìm process đang chiếm giữ Port:**
  ```powershell
  # Cách 1: Sử dụng Get-NetTCPConnection (Khuyến nghị)
  Get-NetTCPConnection -LocalPort 3000 -ErrorAction SilentlyContinue | Select-Object OwningProcess, LocalAddress, LocalPort

  # Cách 2: Sử dụng netstat fallback
  netstat -ano | findstr :3000
  ```
- **Kết thúc Process an toàn:**
  ```powershell
  Stop-Process -Id <PID> -Force
  ```
