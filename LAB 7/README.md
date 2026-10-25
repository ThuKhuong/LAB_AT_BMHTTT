LAB 7 – Xây dựng chính sách Web Protection trên pfSense + Web Proxy
1. Giới thiệu
Bài thực hành LAB 7 thuộc học phần An toàn hệ thống thông tin, tập trung xây dựng hệ thống kiểm soát truy cập website bằng pfSense và máy chủ Web Proxy riêng biệt.
- pfSense: firewall/gateway, quản lý luồng mạng và ngăn truy cập web trực tiếp để tránh bỏ qua proxy.
- Ubuntu Server 24.04 LTS: vận hành Squid, E2Guardian và ClamAV.
- Windows Server AD/DNS: phân giải tên miền thử nghiệm lab7.test.
- DMZ Web Server: chạy Nginx, cung cấp các website và tệp kiểm thử trong môi trường lab.
Lưu ý: Không cài Squid trực tiếp lên pfSense; chỉ thực hiện kiểm thử trên hệ thống máy ảo do người học quản lý.

2. Mục tiêu
- Thiết lập luồng web Client → E2Guardian (8080) → Squid (3128) → đích, qua sự kiểm soát của pfSense.
- Xây dựng chính sách Allow, Hard Block, Warning/Temporary Bypass, Whitelist.
- Kiểm thử cơ chế chặn truy cập web trực tiếp khi người dùng tắt proxy.
- Thực hành quy tắc truy cập theo khung giờ 60 phút (time-window policy).
- Kiểm tra quét tệp tải xuống bằng ClamAV, giới hạn dung lượng và các log tương ứng.
3. Sơ đồ địa chỉ IP
Thành phần	Địa chỉ IP	Vai trò
pfSense – LAN	10.6.0.1	Gateway LAN
pfSense – DMZ	172.16.6.1	Gateway DMZ
pfSense – MGMT	192.168.56.254	Quản trị
DC01	10.6.0.10	AD/DNS
WEB-PROXY	10.6.0.30	E2Guardian + Squid + ClamAV
Client-A	10.6.0.21	Máy kiểm thử
Client-B	10.6.0.22	Máy kiểm thử
DMZ-WEB	172.16.6.10	Nginx / website mô phỏng


Cổng dịch vụ: E2Guardian 10.6.0.30:8080; Squid 127.0.0.1:3128 (chỉ localhost).
4. Các bước thực hiện
1. Kiểm tra kết nối, IP và DNS giữa các máy ảo.
2. Cài đặt và cấu hình Squid, E2Guardian, ClamAV trên WEB-PROXY.
3. Cấu hình Nginx trên DMZ-WEB và tạo các website *.lab7.test.
4. Tạo bản ghi DNS trỏ các domain thử nghiệm về 172.16.6.10.
5. Cấu hình proxy 10.6.0.30:8080 trong trình duyệt máy Client.
6. Tạo alias và firewall rules trên pfSense, chặn truy cập trực tiếp TCP/80 và TCP/443 từ Client-A/B.
7. Tạo danh sách website được phép, bị chặn và cảnh báo trong E2Guardian.
8. Thử từng chính sách, kiểm tra nhật ký pfSense, E2Guardian và Squid.
5. Chính sách website


