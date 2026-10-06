# LAB 3 - THIẾT LẬP MÔ HÌNH TƯỜNG LỬA pfSense

## 1. Mục tiêu
- Xây dựng mô hình pfSense bảo vệ LAN và DMZ.
- Cấu hình WAN, LAN, DMZ, NAT và firewall rule.
- Dùng Windows Server/Domain Controller để kiểm thử lưu lượng.
- Theo yêu cầu giảng viên: **chỉ thực hiện Tình huống 1, 2, 3; bỏ Tình huống 4 và 5**.

## 2. Mô hình mạng
| Thành phần | IP / cấu hình |
|---|---|
| pfSense WAN | DHCP, ưu tiên Bridged Adapter |
| pfSense LAN | 10.0.0.1/8 |
| Domain Controller | 10.0.0.2/8 - GW 10.0.0.1 - DNS 10.0.0.2 |
| Máy thật quản trị | 10.0.0.100/8 - không Gateway/DNS trên Host-only |
| LAN-Test | 10.0.0.3/8 - GW 10.0.0.1 |
| pfSense DMZ | 172.16.0.1/16 |
| DMZ-Web | 172.16.0.2/16 - GW 172.16.0.1 |

## 3. Phần mềm / ISO cần chuẩn bị
- Oracle VirtualBox.
- pfSense CE 2.7.2-RELEASE amd64.
- Windows Server 2019/2022 ISO.
- Ubuntu Server 22.04/24.04 LTS minimal ISO.
- 7-Zip.

## 4. Các máy ảo
- `pfSense`: 2 GB RAM, 2 vCPU, 16-20 GB disk, 3 NIC.
- `Domain-Controller`: Windows Server, khoảng 2 GB RAM.
- `LAN-Test`: Ubuntu Server, khoảng 1 GB RAM.
- `DMZ-Web`: Windows Server, khoảng 2 GB RAM.

## 5. Các bước chính
1. Tạo pfSense và cấu hình 3 card:
   - Adapter 1 = Bridged → WAN
   - Adapter 2 = Host-only → LAN
   - Adapter 3 = Internal Network `dmz-net` → DMZ
2. Cài pfSense, đặt LAN `10.0.0.1/8`, không bật DHCP LAN.
3. Cấu hình Domain Controller `10.0.0.2/8`.
4. Truy cập WebGUI `https://10.0.0.1`.
5. Cấu hình DMZ `172.16.0.1/16`.
6. Kiểm tra Automatic/Hybrid Outbound NAT.
7. Disable Default allow LAN to any IPv4/IPv6, giữ Anti-Lockout.
8. Tạo rule nền tảng `Pass Any | LAN net -> Any`.
9. Kiểm thử và Reset States khi thay đổi ruleset.

## 6. Tình huống 1 - Chặn ICMP, vẫn cho Web/DNS
Thứ tự rule:
1. `Block ICMP | LAN net -> Any`
2. `Pass TCP/UDP 53 | LAN net -> Any`
3. `Pass TCP 80/443 | LAN net -> Any`

Kết quả:
- `ping 8.8.8.8` → thất bại.
- DNS → thành công.
- HTTPS → thành công.

## 7. Tình huống 2 - Chỉ cho một host ra Internet
Thứ tự rule:
1. `Pass Any | 10.0.0.2 -> Any`
2. `Block Any | LAN net -> Any`

Kết quả:
- DC `10.0.0.2` ping Internet → thành công.
- LAN-Test `10.0.0.3` ping Internet → thất bại.

## 8. Tình huống 3 - Cô lập DMZ khỏi LAN
Trước khi Block, tạo baseline:
- `Pass Any | DMZ net -> Any`
- Từ DMZ-Web `172.16.0.2` ping DC `10.0.0.2` → phải thành công.

Sau đó:
1. `Block Any | DMZ net -> LAN net`
2. `Pass Any | DMZ net -> Any`

Kết quả:
- DMZ-Web ping DC → thất bại.
- DMZ-Web vẫn có thể ra Internet nếu rule/DNS đã cấu hình đúng.





