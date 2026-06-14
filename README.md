# bai5_MaNguonMo
BÀI TẬP 5 - HỌC PHẦN PHÁT TRIỂN ỨNG DỤNG MÃ NGUỒN MỞ
Trương Văn Quyến - 
K225480106013
PHẦN I: LÝ THUYẾT
1. Khái niệm Docker là gì?
<img width="886" height="371" alt="image" src="https://github.com/user-attachments/assets/206299b3-4432-4525-8652-60a78665fb59" />
1. Docker là gì?
Docker là một nền tảng mã nguồn mở cho phép các nhà phát triển tự động hóa việc triển khai, quản lý và chạy các ứng dụng bên trong các môi trường biệt lập được gọi là Container (thùng chứa).

Để dễ hình dung, hãy tưởng tượng Docker giống như ngành vận tải biển:

Quá khứ: Hàng hóa (code, thư viện, hệ điều hành) được xếp lộn xộn trên tàu. Khi sang tàu khác hoặc môi trường khác (từ Laptop lên Server), mọi thứ dễ bị xung đột, hỏng hóc.

Với Docker: Mọi thứ ứng dụng cần để chạy được đóng gói gọn gàng vào một chiếc Container. Chiếc container này chạy giống hệt nhau trên bất kỳ boong tàu (máy tính) nào có cài đặt Docker.
2. Các từ khóa phổ biến trong docker-compose.yml
docker-compose.yml là file cấu hình (định dạng YAML) dùng để định nghĩa và chạy các ứng dụng Docker gồm nhiều container cùng lúc. Dưới đây là các từ khóa cốt lõi phân theo từng thành phần:

Cấu trúc tổng thể (Top-level định nghĩa)
version: (Hiện tại là tùy chọn trong các bản Docker mới) Xác định phiên bản định dạng của file Compose.

services: Nơi định nghĩa các container (ứng dụng) sẽ chạy.

networks: Định nghĩa mạng ảo để các container giao tiếp với nhau.

volumes: Định nghĩa vùng lưu trữ dữ liệu bền vững (không bị mất khi container xóa).

Từ khóa mô tả một service (Container)
image: Chỉ định Docker Image được dùng để khởi tạo container (ví dụ: postgres:15, nginx:latest).

build: Dùng khi bạn muốn Docker tự build image từ một Dockerfile có sẵn trong máy thay vì tải từ internet.

ports: Ánh xạ cổng (port) từ máy thật (Host) vào trong container theo cú pháp HOST:CONTAINER.

environment: Thiết lập các biến môi trường cho ứng dụng bên trong container.

depends_on: Chỉ định thứ tự khởi động giữa các service (ví dụ: app chỉ chạy sau khi database đã khởi động).

volumes (cấp độ service): Gắn một thư mục từ máy thật hoặc một volume được định nghĩa chung vào container.

networks (cấp độ service): Chỉ định service này thuộc mạng nào để kết nối nội bộ.
