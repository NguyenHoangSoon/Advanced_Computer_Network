# Hướng dẫn sử dụng GitHub để làm việc nhóm với file Cisco Packet Tracer (.pka)

## Mục tiêu

Hướng dẫn này giúp nhóm 3 người làm việc **hiệu quả và an toàn trên GitHub** với file `.pka` của Cisco Packet Tracer, đảm bảo mỗi người cấu hình nối tiếp mà không bị mất dữ liệu.

---

## 1. Chuẩn bị ban đầu

### 1.1. Clone repo

Clone repo về máy:

   ```bash
   git clone https://github.com/NguyenHoangSoon/Advanced_Computer_Network.git
   ```

### 1.2. Cấu trúc thư mục đề xuất

```
Advanced_Computer_Network/
│
├── pka/                # Chứa các file .pka theo phiên bản
│   ├── BuildBranch_NguyenHoangSon_2410.pka
│   ├── ConfigVPN_TranQuangThai_2510.pka
│   └── ConfigDHCP_PhanDucTai_2610.pka
│
├── configs/            # Xuất file cấu hình (show run)
│   ├── Showrun_BuildBranch_NguyenHoangSon_2410.txt
│   ├── Showrun_ConfigVPN_TranQuangThai_2510.txt
│   └── Showrun_ConfigDHCP_PhanDucTai_2610.txt

│
├── scripts/               # Nếu có script tự động cấu hình VPN
│   └── script_NguyenHoangSon_2410.py
│
└── README.md           # Tài liệu hướng dẫn chính (file này)
```

---

## 2. Quy trình làm việc nhóm thực tế

### Bước 1 — Người đầu tiên (A) tạo file .pka

1. Mở **Cisco Packet Tracer** → thiết kế topology + cấu hình ban đầu.
2. Lưu file vào thư mục `pka/` → ví dụ: `BuildBranch_NguyenHoangSon_2410.pka`.
3. Commit và push lên GitHub:

   ```bash
   git add .
   git commit -m "A: Xay dung so do chi nhanh"
   git push origin main
   ```

### Bước 2 — Thành viên tiếp theo (B) cập nhật cấu hình

1. Luôn **pull bản mới nhất về trước khi làm**:

   ```bash
   git pull origin main
   ```
2. **Copy 1 file backup:** 

Tạo 1 bản copy của file vừa pull về, sau đó cấu hình trên file gốc. 

- Ví dụ: copy file `BuildBranch_NguyenHoangSon_2410.pka` thành file `Copy of BuildBranch_NguyenHoangSon_2410.pka`
- Đổi tên file gốc `BuildBranch_NguyenHoangSon_2410.pka` thành `BuildBranch_TranQuangThai_2510.pka`và cấu hình tiếp trên file này
- Đối với file copy `Copy of BuildBranch_NguyenHoangSon_2410.pka`, sửa tên lại thành file gốc `BuildBranch_NguyenHoangSon_2410.pka`-> đây sẽ là version cũ để sau này backup. Đỡ tốn thời gian lục các commit trên github
- Làm tương tự với các file sau

3. Commit và push:

   ```bash
   git add .
   git commit -m "B: Tiep tuc xay dung so do branch"
   git push origin main
   ```

### Bước 3 — Thành viên C cập nhật thêm NAT, DHCP
Làm tương tự bước 2


## 3. Quy tắc quan trọng để tránh lỗi

Quy tắc:                               
- Luôn `git pull` trước khi làm
- Chỉ một người làm tại một thời điểm 
- Đặt tên file theo đúng cú pháp   
- Trong quá trình làm: cấu hình - ghi lại lệnh + hình output
- Sau khi xong hết: `show runnning config`, copy tất cả và dán vào thư mục config log
- LUÔN NHỚ:
1. **GHI LỆNH CẤU HÌNH KÈM HÌNH ẢNH VÀO FILE DOC VÀ ĐẶT VÀO THƯ MỤC report**         
2. **GHI LẠI LỆNH CẤU HÌNH (SHOW RUN_TÊN_NGÀY) VÀO THƯ MỤC config log** 

ĐỪNG QUÊN, ĐỪNG QUÊN, ĐỪNG QUÊN!!!
---


