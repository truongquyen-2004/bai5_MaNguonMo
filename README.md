# bai5_MaNguonMo
BÀI TẬP 5 - HỌC PHẦN PHÁT TRIỂN ỨNG DỤNG MÃ NGUỒN MỞ
- Trương Văn Quyến 
- K225480106013
- Lớp:K58-KTP-K01
  
PHẦN 1: LÝ THUYẾT
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

3. Ưu điểm khi triển khai ứng dụng sử dụng Docker
Tính nhất quán (Consistent Environment): Giải quyết triệt để câu nói kinh điển "Code chạy ngon trên máy em nhưng lên server lại lỗi". App chạy ở laptop thế nào thì lên server sẽ y hệt như vậy.

Nhẹ và Hiệu năng cao (Lightweight): Khác với máy ảo (VM) cần cài nguyên một hệ điều hành ảo hóa kèm theo, Docker Container chia sẻ chung nhân (kernel) của hệ điều hành máy host nên khởi động chỉ mất vài giây và tốn cực ít tài nguyên (RAM/CPU).

Dễ dàng mở rộng (Scalability): Bạn có thể tăng từ 1 container lên 5, 10 container chỉ bằng một câu lệnh để gánh tải cho hệ thống.

Cô lập an toàn (Isolation): Mỗi container là một môi trường độc lập. Nếu một ứng dụng bị hack hoặc crash, nó không làm ảnh hưởng trực tiếp đến các ứng dụng khác trên cùng một máy chủ.

Quản lý phiên bản trực quan: Bạn có thể dễ dàng rollback (quay lại) phiên bản cũ của app chỉ bằng cách thay đổi tag của image (ví dụ từ v2 về v1).

4. Lý thuyết triển khai app Docker lên Máy chủ thật KHÔNG CÓ INTERNET (Offline Deployment)Khi máy chủ đích (Production Server) bị cô lập hoàn toàn với internet (môi trường Air-gapped), bạn không thể dùng lệnh docker pull hay docker-compose up để tự tải image từ mạng về được. Quy trình xử lý về mặt lý thuyết sẽ trải qua 3 giai đoạn chính: Đóng gói tại Máy cá nhân $\rightarrow$ Di chuyển vật lý $\rightarrow$ Bung lụa tại Máy chủ.
##  Bước 1: Chuẩn bị và Đóng gói (Thực hiện trên Laptop có internet)
1 Build ứng dụng hoàn chỉnh: Chạy lệnh build để tạo ra các Docker Image hoàn chỉnh của app trên laptop. Đảm bảo mọi thứ đã test OK.

2 Trích xuất Image thành file nén (Archive): Sử dụng tính năng docker save của Docker để đóng gói toàn bộ Image (bao gồm cả các layer cấu thành và thư viện) thành một file vật lý (thường là định dạng .tar).

Ví dụ: Đóng gói image my-app:v1 thành file my-app-v1.tar.

3 Tải Docker cài đặt offline (nếu Server chưa cài Docker): Bạn cần tải sẵn file cài đặt Docker dạng Binary hoặc các gói package (.deb cho Ubuntu hoặc .rpm cho CentOS) tương ứng với hệ điều hành của Server để mang theo.
##  Bước 2: Di chuyển dữ liệu (Vận chuyển vật lý)
1 Copy toàn bộ các file sau vào một thiết bị lưu trữ ngoại vi (USB, ổ cứng di động, hoặc đĩa CD/DVD tùy thuộc vào quy định bảo mật của trung tâm dữ liệu):

Các file .tar chứa Docker Images.

File cấu hình docker-compose.yml.

 Bộ cài đặt Docker Offline (nếu cần).

2 Mang thiết bị này đến cắm trực tiếp vào máy chủ không có internet và copy dữ liệu vào bộ nhớ máy chủ
##  Bước 3: Triển khai và Kích hoạt (Thực hiện trên Máy chủ không internet)
1 Cài đặt Docker Engine: Sử dụng các gói cài đặt offline đã chuẩn bị để cài đặt và bật dịch vụ Docker trên máy chủ.

2 Nạp Image vào Docker hệ thống (Import): Sử dụng lệnh docker load để giải nén file .tar ngược trở lại thành các Docker Image nằm trong bộ nhớ local của máy chủ.

Lúc này, lệnh docker images trên server sẽ hiển thị danh sách các image y hệt như trên laptop của bạn.

3 Khởi chạy ứng dụng: Kiểm tra lại file docker-compose.yml (đảm bảo các đường dẫn volume, cấu hình cổng phù hợp với máy chủ), sau đó chạy lệnh docker-compose up -d. Docker sẽ lấy ngay các image vừa được nạp vào máy để chạy mà không cần kết nối ra internet.
