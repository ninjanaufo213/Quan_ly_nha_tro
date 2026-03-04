🏠 Room Management System - Deployment & Operations
Dự án này sử dụng kiến trúc Microservices được đóng gói bằng Docker, tự động hóa triển khai qua GitHub Actions (CI/CD) lên server AWS EC2.

🚀 Quy trình Vận hành (DevOps Stack)
1. Kiến trúc Docker
Hệ thống bao gồm 3 dịch vụ chính được quản lý bởi docker-compose:
Frontend: React/Next.js (Port 3000)
Backend: FastAPI (Port 8000)
Database: MySQL 8.0 (Port 3306 nội bộ, 3307 bên ngoài)
2. CI/CD Pipeline (GitHub Actions)
Mỗi khi có code mới được push lên branch main:
Build: GitHub Actions tự động build Docker Images cho Frontend và Backend.
Push: Đẩy Images lên Docker Hub (vinhcute22/room-frontend, vinhcute22/room-backend).
Deploy:
Sử dụng SSH để kết nối vào EC2.
Thực hiện lệnh docker compose pull để lấy bản mới nhất.
Khởi động lại các services.
🛠 Cấu hình Tối ưu cho Server yếu (t3.micro - 1GB RAM)
Do tài nguyên trên EC2 hạn chế, dự án cần áp dụng các kỹ thuật sau để tránh lỗi OOM (Out of Memory - Error 137):

⚡ Tạo RAM ảo (Swap File)
Bắt buộc thực hiện trên EC2 để hỗ trợ MySQL 8.0 khởi tạo:
Bash
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

📉 Tạo server trên aws (Sử dụng dịch vụ EC2) : Chọn loại t3.micro, hệ điều hành Ubuntu 24.04 LTS, Storage : 15GiB

🔐 Cấu hình Bảo mật & Kết nối
Tạo key pair : File .pem chỉ lưu tại máy mình giúp tăng cường bảo mật chống kẻ lạ truy cập server.
AWS Security Group
Để ứng dụng hoạt động, cần mở các cổng Inbound sau trên AWS Console:
3000: Truy cập giao diện người dùng.
8000: API Backend (Phải mở để Frontend gọi được dữ liệu).
22: SSH Quản trị.

🤖 Automation with Ansible
Dự án sử dụng Ansible để quản lý cấu hình và triển khai tự động (Infrastructure as Code). Thay vì thao tác thủ công, mọi thiết lập trên EC2 đều được định nghĩa trong các file YAML, đảm bảo tính đồng nhất giữa các lần triển khai.
1. Cấu trúc thư mục Ansible
<img width="995" height="371" alt="image" src="https://github.com/user-attachments/assets/9582f315-7350-4368-80f0-dc7668ec3cae" />

2. Các Task tự động hóa chính
Cài đặt môi trường: Tự động cài đặt Docker, Docker Compose và các thư viện hệ thống cần thiết.
