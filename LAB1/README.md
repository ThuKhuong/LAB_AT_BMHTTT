# LAB 1 - BẮT GÓI TIN TELNET - SSH

## Thông tin sinh viên

- **Họ và tên:** Lê Thu Khương
- **Mã số sinh viên:** 1150080142
- **Tên bài Lab:** Lab 1 - Bắt gói tin Telnet - SSH

## Mục tiêu

- Thực hiện kết nối Telnet và SSH giữa Client và Server.
- Sử dụng Wireshark để bắt và xem các gói tin.
- So sánh sự khác nhau giữa Telnet và SSH.

## Môi trường thực hiện

- **Máy ảo:** Oracle VirtualBox
- **Server:** Ubuntu Server 26.04 LTS
- **Client:** Windows
- **Công cụ:** Wireshark, PuTTY
- **Tài khoản thử nghiệm:** `uitlab`

## Nội dung thực hiện

### 1. Telnet

Tạo tài khoản:

```bash
sudo adduser uitlab
```

Cài đặt Telnet:

```bash
sudo apt update
sudo apt install inetutils-telnetd inetutils-inetd update-inetd -y
```

Kiểm tra cổng 23:

```bash
ss -ltn | grep ':23'
```

Sử dụng bộ lọc sau trên Wireshark để bắt gói Telnet:

```text
tcp.port == 23
```

Sau khi kết nối Telnet, thực hiện một số lệnh:

```bash
pwd
ls
mkdir test
ls
```

Đổi mật khẩu tài khoản:

```bash
sudo passwd uitlab
```

Sau đó kết nối lại Telnet và tiếp tục bắt gói tin bằng Wireshark.

### 2. SSH

Cài đặt SSH Server:

```bash
sudo apt install openssh-server -y
sudo systemctl enable --now ssh
```

Kiểm tra trạng thái SSH:

```bash
systemctl status ssh
```

Sử dụng bộ lọc sau trên Wireshark:

```text
tcp.port == 22
```

Sau khi kết nối SSH, thực hiện:

```bash
pwd
ls
mkdir ssh_test
ls
```

### 3. Đăng nhập SSH bằng Public Key

Tạo SSH key trên Windows:

```powershell
ssh-keygen -t ed25519
```

Sau khi thêm public key vào Server, thực hiện kết nối:

```powershell
ssh uitlab@192.168.56.1
```

## Kết quả thực hiện

Đã kết nối được từ Client đến Ubuntu Server bằng Telnet và SSH.

Khi bắt gói Telnet bằng Wireshark, có thể quan sát được nội dung trao đổi nếu bắt đúng lưu lượng. Điều này cho thấy Telnet không bảo vệ tốt dữ liệu truyền trên mạng.

Đối với SSH, Wireshark vẫn bắt được các gói tin trên cổng 22 nhưng nội dung bên trong đã được mã hóa nên không thể đọc trực tiếp như Telnet.

Ngoài cách đăng nhập bằng mật khẩu, đã thực hiện thử đăng nhập SSH bằng public key.

## Video thực hành

**YouTube:** https://youtu.be/L3k_yDnAaIg?si=OG--3jiseyz2sCN6

## Lưu ý

- Kiểm tra IP của Ubuntu Server bằng lệnh `ip a` trước khi kết nối.
- Telnet sử dụng cổng TCP/23.
- SSH sử dụng cổng TCP/22.
- Khi sử dụng Wireshark cần chọn đúng interface mạng để bắt được gói tin.
