# LAB 3 – Nhận diện và ứng phó các mối đe dọa đến an toàn thông tin

**Họ tên:** Lê Thu Khương
**MSSV:** 1150080142
**Lớp:** 11_DH_CNPM2  
**Năm học:** 2026–2027  

## 1. Môi trường

| Thành phần | Phiên bản yêu cầu | Trạng thái |
|---|---|---|
| VMware Workstation Pro | 26H1 | [CHƯA XÁC MINH] |
| Windows 11 | 25H2 x64, build 26200.9445 | [CHƯA XÁC MINH] |
| Microsoft Defender | Real-time + Tamper Protection bật | [CHƯA XÁC MINH] |
| Sysmon | 15.22 | [CHƯA XÁC MINH] |
| Autoruns | 14.3 | [CHƯA XÁC MINH] |
| Process Explorer | 17.14 | [CHƯA XÁC MINH] |
| Wireshark + Npcap | 4.6.8 | [CHƯA XÁC MINH] |
| Python | 3.14.7 | [CHƯA XÁC MINH] |

## 2. Cấu trúc thư mục

```text
C:\LAB3\
├── Evidence
├── Tools
│   ├── Sysmon
│   ├── Autoruns
│   └── ProcessExplorer
├── Downloads
└── lab3_assets
    ├── data
    ├── samples
    ├── scripts
    ├── www
    ├── README.txt
    └── sysmon-lab.xml
```

## 3. Các tình huống thực hiện

| Tình huống | Nội dung | Kết quả |
|---|---|---|
| Baseline | OS, Defender, Firewall, Network, Process | [CHƯA XÁC MINH] |
| TH1 | Risk register và phân loại nguồn đe dọa | Nội dung đã chuẩn bị |
| TH2 | EICAR + Defender detection/quarantine | [CHƯA XÁC MINH] |
| TH3 | Event 4624/4625/4648 + password rotation | [CHƯA XÁC MINH] |
| TH4 | Sysmon + Autoruns + persistence + listener localhost | [CHƯA XÁC MINH] |
| TH5 | HTTP plaintext vs HTTPS/TLS | [CHƯA XÁC MINH] |
| TH6 | Local load + DDoS dataset + mail bombing log | [CHƯA XÁC MINH] |
| TH7 | Phishing + Social Engineering offline | [CHƯA XÁC MINH] |
| Cleanup | Xóa artefact, dừng listener, hash Evidence | [CHƯA XÁC MINH] |

## 4. Bằng chứng ảnh

- `H1_VM_WindowsVersion.png`
- `H2_ToolVersions.png`
- `H3_Baseline_Defender_Firewall.png`
- `H4_ProtectionHistory_EICAR.png`
- `H5_Event4625.png`
- `H6_Sysmon_Event1.png`
- `H7_Autoruns_LAB3_Run_Demo.png`
- `H8_ProcessExplorer_Python.png`
- `H9_HTTP_Plaintext.png`
- `H10a_TLS_443.png`
- `H10b_Load_and_Log_Analysis.png`
- `H10c_Phishing_Offline.png`
- `H11_Recovery_Verification.png`

> Tài liệu gốc dùng trùng số H10 ở một số tình huống; README này dùng H10a/H10b/H10c để tránh nhầm tên file.

## 5. Output/log cần lưu

Các file đầu ra nên nằm trong `Evidence/`, ví dụ:

```text
start_time.txt
baseline_os.txt
baseline_defender.txt
baseline_firewall.txt
baseline_network.txt
baseline_processes.txt
defender_eicar.txt
auth_events_before_rotation.txt
autoruns_before.csv
sysmon_persistence.txt
task_ran.txt
local_load_test.txt
ddos_sources.txt
mail_sender_counts.txt
mail_volume.txt
autoruns_after.csv
autoruns_diff.txt
evidence_sha256.csv
```

## 6. PASS/FAIL

Chỉ đổi thành `PASS` sau khi có bằng chứng thật:

- [ ] TH1: Có ≥5 mục risk register và phân loại 5 nguồn đe dọa.
- [ ] TH2: Có Defender detection/quarantine EICAR.
- [ ] TH3: Có 4624/4648, 4625 và credential cũ fail sau rotation.
- [ ] TH4: Phát hiện `LAB3_Run_Demo`, `LAB3_Persistence_Demo`; listener chỉ `127.0.0.1:8080`; PID được tương quan.
- [ ] TH5: Có capture HTTP đọc được `TRAINING_ONLY` và TLS/443 để so sánh; không làm MITM chủ động.
- [ ] TH6: Load test chỉ nhắm `127.0.0.1:8080`; phân tích được DDoS/mail log.
- [ ] TH7: Nhận diện ≥5 chỉ dấu phishing và phân loại 6 case.
- [ ] Cleanup: Không còn persistence/listener, Defender vẫn bật, Evidence đã SHA-256.

## 7. Lỗi gặp phải và cách khắc phục

Điền theo thực tế, ví dụ:

- `[Lỗi] Shared Folder chưa xuất hiện trong Windows 11 VM`  
  `[Khắc phục] Cài VMware Tools, bật Guest Isolation và Shared Folders.`

- `[Lỗi khác] ...`  
  `[Khắc phục] ...`

## 8. Lưu ý an toàn

- Không tắt Defender/Tamper Protection để làm lab.
- Không tạo exclusion để cho EICAR chạy.
- Không sửa `local_load_test.py` sang mục tiêu khác `127.0.0.1:8080`.
- Không thực hiện DDoS, mail bombing, spoofing hoặc MITM chủ động trên mạng ngoài VM.
- Không commit password, token/API key, cookie/session, email thật hoặc log chưa làm sạch.
- Không commit installer/executable Sysinternals, Wireshark, Python hoặc file bị Defender quarantine.

## 9. Cấu trúc repository đề nghị

```text
LAB_AT_BMHTTT/
└── LAB3/
    ├── README.md
    ├── 11_DH_CNPM2-LAB3_1150080142_LeThuKhuong.docx
    ├── Evidence/
    │   └── (output/log đã làm sạch)
    └── evidence_sha256.csv
```

## 10. Trạng thái trước khi nộp

- [ ] Chèn ảnh thật vào báo cáo Word.
- [ ] Xóa toàn bộ placeholder `[CHƯA XÁC MINH]`.
- [ ] Đối chiếu timestamp ảnh với log/output.
- [ ] Tạo `evidence_sha256.csv`.
- [ ] Cleanup và chụp H11.
- [ ] Push repo ở chế độ Public.
- [ ] Mở URL repo trong cửa sổ không đăng nhập để kiểm tra truy cập.
