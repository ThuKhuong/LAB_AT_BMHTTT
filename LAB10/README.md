# LAB 10 - Remote DNS Cache Poisoning
## 1. Mục tiêu
Tìm hiểu cơ chế Remote DNS Cache Poisoning trong môi trường mạng pfSense cô lập và cách bảo vệ DNS Resolver.
## 2. Môi trường thực hành
- Ubuntu Server: DNS Server (10.9.0.10)
- Kali Linux: Attacker (10.10.0.200)
- pfSense: Firewall và phân chia mạng
- BIND9: DNS Resolver
## 3. Nội dung thực hiện
- Cấu hình môi trường mạng và DNS Server.
- Kiểm tra khả năng phân giải tên miền.
- Tìm hiểu cơ chế DNS Cache Poisoning.
- Kiểm tra kết quả phân giải DNS và truy cập HTTP.
- Phân tích các biện pháp bảo vệ DNS.
## 4. Kết quả
- DNS phân giải tên miền `www.example.lab10.test` về IP `10.10.0.200`.
- Kiểm tra HTTP tại `10.10.0.200` trả về `200 OK`.
- Kết quả cho thấy DNS và Web Server mô phỏng có thể phản hồi yêu cầu.
## 5. Kết luận
Qua bài thực hành, em hiểu rõ hơn về cơ chế phân giải DNS, nguy cơ DNS Cache Poisoning và tầm quan trọng của việc bảo vệ DNS Resolver.
