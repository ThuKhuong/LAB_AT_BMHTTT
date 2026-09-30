# LAB 4 – Khảo sát và đánh giá bề mặt mạng bằng Nmap

## 1. Mục tiêu
- Thiết lập môi trường thực hành mạng cô lập với Kali Linux và Metasploitable 2.
- Sử dụng Nmap để phát hiện máy đích, khảo sát trạng thái cổng, nhận diện dịch vụ và hệ điều hành.
- Tìm hiểu sự khác biệt giữa các kỹ thuật quét TCP/UDP và kết quả của NSE.
- So sánh kết quả quét trước và sau khi thay đổi cấu hình bảo mật (hardening).

> **Lưu ý:** Chỉ thực hiện quét trên các máy ảo trong môi trường LAB được phép. Metasploitable 2 là máy cố ý chứa lỗ hổng; không kết nối Bridged hoặc đưa ra mạng công cộng.

## 2. Môi trường và công cụ
| Thành phần | Vai trò |
|---|---|
| Windows | Máy thật, cài VirtualBox; có thể cài Nmap Windows |
| Oracle VirtualBox | Tạo và quản lý máy ảo, mạng Host-Only |
| Kali Linux | Máy thực hiện quét bằng Nmap |
| Metasploitable 2 | Máy đích để khảo sát trong LAB |
| Nmap / NSE | Công cụ quét cổng, nhận diện dịch vụ và chạy script kiểm tra |

Tải công cụ từ nguồn chính thức:
- VirtualBox: https://www.virtualbox.org/wiki/Downloads
- Kali Linux (Pre-built VMs → VirtualBox): https://www.kali.org/get-kali/
- Metasploitable 2: https://docs.rapid7.com/metasploit/metasploitable-2/
- Nmap: https://nmap.org/download

## 3. Chuẩn bị và cấu hình mạng
1. Thêm máy ảo Kali vào VirtualBox bằng file `.vbox` của bộ Pre-built VM.
2. Tạo máy ảo **Metasploitable2** (Linux/Ubuntu 32-bit), chọn **Use an Existing Virtual Hard Disk File** và gắn file `Metasploitable.vmdk` đã giải nén.
3. Tắt hai máy ảo trước khi chỉnh mạng. Tạo/chọn cùng một **Host-Only Network** trong VirtualBox.
4. Cấu hình adapter của Kali và Metasploitable 2 vào **cùng mạng Host-Only**. Nếu Kali dùng NAT tạm để cập nhật gói, ngắt NAT trước khi quét theo yêu cầu LAB.
5. Khởi động hai máy, xác định IP và kiểm tra khả năng liên lạc.

**Ảnh nên chụp:** danh sách 2 VM; cấu hình Network của từng VM; địa chỉ IP và kết quả kiểm tra kết nối.

### Lệnh chuẩn bị trên Kali
```bash
sudo apt update
sudo apt install -y nmap
nmap --version
ip a
```

Trên Metasploitable 2, xem địa chỉ IP:
```bash
ifconfig
```

Trên Kali, kiểm tra kết nối (thay IP thực tế của máy đích):
```bash
ping -c 4 <IP_METASPLOITABLE>
```

## 4. Các bước thực hành Nmap
**Thay `<IP_METASPLOITABLE>` bằng IP máy đích trong LAB.** Ghi lại câu lệnh, thời điểm, địa chỉ IP và kết quả thực tế. Các lệnh dưới đây là mẫu để tổ chức thực hành; đối chiếu yêu cầu cụ thể trong tài liệu giảng viên trước khi nộp.

### 4.1. Phát hiện máy và khảo sát cổng
```bash
sudo nmap -sn <IP_METASPLOITABLE>
sudo nmap <IP_METASPLOITABLE>
sudo nmap -p 1-1000 <IP_METASPLOITABLE>
```
**Chụp ảnh:** kết quả phát hiện host và danh sách cổng với trạng thái `open`, `closed` hoặc `filtered` (nếu có).

### 4.2. So sánh các kiểu quét TCP
```bash
sudo nmap -sS <IP_METASPLOITABLE>
nmap -sT <IP_METASPLOITABLE>
sudo nmap -sA <IP_METASPLOITABLE>
sudo nmap -sF <IP_METASPLOITABLE>
sudo nmap -sX <IP_METASPLOITABLE>
sudo nmap -sN <IP_METASPLOITABLE>
```
**Chụp ảnh:** kết quả SYN, TCP Connect, ACK và một kết quả FIN/Xmas/NULL để phân tích sự khác biệt.

### 4.3. UDP, phiên bản dịch vụ và hệ điều hành
```bash
sudo nmap -sU --top-ports 20 <IP_METASPLOITABLE>
sudo nmap -sV <IP_METASPLOITABLE>
sudo nmap -O <IP_METASPLOITABLE>
```
**Chụp ảnh:** trạng thái UDP; tên/phiên bản dịch vụ; kết quả nhận diện hệ điều hành (nếu có).

### 4.4. NSE – kiểm tra bằng script
Chỉ chạy script được phép trong phạm vi bài LAB. Ví dụ nhóm script mặc định:
```bash
sudo nmap -sC <IP_METASPLOITABLE>
```
**Chụp ảnh:** tên script và đầu ra thực tế; ghi rõ nếu script lỗi hoặc timeout. Timeout **không** chứng minh hệ thống không có lỗ hổng.

### 4.5. Lưu kết quả để đối chiếu
```bash
mkdir -p ~/LAB4/results
sudo nmap -sS -sV -oN ~/LAB4/results/before.txt <IP_METASPLOITABLE>
```
Sau khi thay đổi một cấu hình bảo mật trong môi trường LAB, chạy **đúng lệnh và đúng mục tiêu** để so sánh:
```bash
sudo nmap -sS -sV -oN ~/LAB4/results/after.txt <IP_METASPLOITABLE>
diff -u ~/LAB4/results/before.txt ~/LAB4/results/after.txt
```
**Chụp ảnh:** cấu hình đã thay đổi, kết quả `before` và `after`. Chỉ nhận xét sự thay đổi thực sự xuất hiện trong kết quả của bạn.

## 5. Nội dung phân tích cần trình bày
1. **Open / closed / filtered:** Open có dịch vụ lắng nghe; closed không có dịch vụ lắng nghe nhưng máy đích phản hồi; filtered là trạng thái không xác định được do lọc gói hoặc thiếu phản hồi phù hợp.
2. **`-sS` và `-sT`:** SYN Scan thao tác với gói tin thô nên thường cần quyền cao; TCP Connect Scan dùng lời gọi kết nối của hệ điều hành.
3. **FIN / Xmas / NULL:** Phản hồi thay đổi theo TCP/IP stack và firewall; `open|filtered` không có nghĩa chắc chắn cổng mở.
4. **ACK và SYN:** SYN giúp khảo sát trạng thái cổng; ACK chủ yếu khảo sát việc lọc gói. `unfiltered` không đồng nghĩa `open`.
5. **UDP:** Không có bắt tay TCP; thiếu phản hồi và giới hạn ICMP khiến quét chậm, dễ xuất hiện `open|filtered`.
6. **`-sV`:** Hỗ trợ nhận diện dịch vụ/phiên bản để đối chiếu bản vá, nhưng kết quả nhận diện có thể thiếu hoặc sai.
7. **OS fingerprinting:** Bị ảnh hưởng bởi firewall, NAT, VM và số lượng cổng quan sát được; cần kiểm chứng thêm.
8. **NSE timeout:** Có thể do mạng, tường lửa, tải hệ thống hoặc script; không thể suy ra máy không có lỗ hổng.
9. **Before / after:** Dùng cùng lệnh, cùng mục tiêu và điều kiện tương đương; kết hợp kết quả quét với bằng chứng thay đổi cấu hình.
10. **Giảm bề mặt tấn công:** Tắt dịch vụ không cần thiết; giới hạn IP/cổng bằng firewall; cập nhật bản vá và tăng cường xác thực/phạm vi truy cập.
