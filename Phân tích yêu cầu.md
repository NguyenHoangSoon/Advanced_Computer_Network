# Tổng quan đề tài: Mạng kết nối chi nhánh qua VPN (Branch Connectivity via VPN)

## 1. Giới thiệu đề tài

### Mục tiêu chính

Mục tiêu của đề tài là **xây dựng một kết nối bảo mật giữa hai chi nhánh doanh nghiệp qua Internet bằng công nghệ Site-to-Site VPN (Virtual Private Network)**.

Trong bối cảnh thực tế, các doanh nghiệp thường có nhiều chi nhánh (branch offices) phân bố khác nhau về địa lý, và cần truy cập tài nguyên nội bộ (database, application server, file sharing) một cách an toàn. Việc sử dụng **VPN** giúp:

* **Mã hóa dữ liệu** khi truyền qua Internet, đảm bảo tính bí mật.
* **Xác thực nguồn gốc** (authentication) giữa các router biên.
* **Giữ nguyên cấu trúc mạng nội bộ** như LAN-to-LAN, mà không cần đến đường truyền riêng.

### Kết quả mong đợi

* Hoàn thiện mô hình kết nối 2 chi nhánh doanh nghiệp qua Internet.
* Cài đặt và cấu hình Site-to-Site VPN (IPsec).
* Đảm bảo truy cập LAN-to-LAN bị mã hóa và an toàn.
* Cài đặt định tuyến, NAT/PAT, DHCP.
* Kiểm tra kết nối, đánh giá hiệu năng và độ ổn định.

---

## 2. Nội dung công việc

### 2.1 Yêu cầu cụ thể

1. Xây dựng mô hình gồm **2 chi nhánh (Branch HCM & Branch NT (Nha Trang))** kết nối qua Internet.
2. Cấu hình **định tuyến tĩnh hoặc OSPF** giữa các chi nhánh.
3. Thiết lập **Site-to-Site VPN (IPsec)** để bảo mật dữ liệu.
4. Cấu hình **NAT, PAT, và DHCP** để cung cấp địa chỉ IP cho các host.
5. **Kiểm tra khả năng truy cập** LAN-to-LAN và độ ổn định kết nối VPN.
6. (Tùy chọn nâng cao) Viết **script tự động cấu hình VPN tunnel**.

### 2.2 Phạm vi

* Mô phỏng trong **Cisco Packet Tracer**.
* Giới hạn 2 router chính (mỗi chi nhánh), một router Internet giả lập.
* Mỗi chi nhánh có LAN gồm router, switch và 1–2 PC.

---

## 3. Kiến thức lý thuyết cần nắm

### 3.1. Kiến thức nền về mạng (Network Fundamentals)

* Phân biệt các loại mạng: LAN, WAN, Internet.
* Các giao thức mạng cơ bản: IP, ICMP, ARP, DNS, DHCP.
* Phân chia địa chỉ IP (subnetting) và định tuyến cơ bản (static/dynamic).

### 3.2. Định tuyến (Routing)

* **Static routing:** cấu hình tuyến thủ công giữa các router.
* **OSPF (Open Shortest Path First):** giao thức định tuyến động dạng link-state, thích hợp trong mô hình nhiều subnet.
* Lệnh cơ bản:

  ```
  router ospf 1
  network 10.1.1.0 0.0.0.255 area 0
  ```

### 3.3. NAT, PAT, DHCP

* **NAT (Network Address Translation):** chuyển đổi địa chỉ private ↔ public để truy cập Internet.
* **PAT (Port Address Translation):** nhiều host dùng chung 1 địa chỉ IP công cộng.
* **DHCP:** cấp phát IP động cho client.

### 3.4. VPN và IPsec

* **VPN (Virtual Private Network):** tạo kênh truyền ảo, bảo mật giữa hai điểm qua Internet.
* **Site-to-Site VPN:** kết nối LAN-to-LAN qua VPN Tunnel.
* **IPsec (Internet Protocol Security):** chuẩn mã hóa, xác thực dữ liệu cho VPN.
* Hai pha thiết lập:

  * **IKE Phase 1:** tạo kênh bảo mật ban đầu (ISAKMP SA).
  * **IKE Phase 2:** tạo kênh dữ liệu (IPsec SA) để truyền tải thật.
* **Thuật toán bảo mật:**

  * Mã hóa: AES, DES
  * Xác thực: SHA, MD5
  * Diffie-Hellman Groups: DH Group 2, 14

### 3.5. Cấu trúc IPsec cơ bản

```
crypto isakmp policy 10
 encr aes
 hash sha
 authentication pre-share
 group 2
 lifetime 86400
crypto isakmp key vpnkey address 203.0.113.2
access-list 100 permit ip 10.1.1.0 0.0.0.255 172.16.1.0 0.0.0.255
crypto ipsec transform-set TS esp-aes esp-sha-hmac
crypto map CMAP 10 ipsec-isakmp
 set peer 203.0.113.2
 set transform-set TS
 match address 100
interface GigabitEthernet0/0
 crypto map CMAP
```

### 3.6. Kiểm thử và giám sát

* **Ping/Traceroute:** kiểm tra kết nối end-to-end.
* **Lệnh show:**

  ```
  show crypto isakmp sa
  show crypto ipsec sa
  show ip route
  show ip nat translations
  ```

---

## 4. Các bước thực hiện chính

1. **Thiết kế sơ đồ mạng và lập kế hoạch IP.**
2. **Cấu hình cơ bản:** interfaces, hostname, routing, NAT, DHCP.
3. **Thiết lập VPN:** tạo IKE Phase 1 & 2, ACL, crypto map.
4. **Kiểm tra kết nối:** ping, show logs, giám sát tunnel.
5. **Ghi nhận kết quả, chụp màn hình, viết báo cáo.**
6. **(Tùy chọn)** tạo script cấu hình tự động.

---

## 5. Kết quả mong đợi khi hoàn thành

* Hai chi nhánh kết nối bảo mật thông qua Site-to-Site VPN.
* Máy trạm LAN ở hai bên truy cập qua lại được.
* NAT, PAT, DHCP hoạt động ổn định.
* Báo cáo có đầy đủ sơ đồ, cấu hình, kết quả kiểm thử.

---

## 6. Đề xuất hướng phát triển mở rộng

* Tích hợp thêm **GRE over IPsec** (kết hợp tính năng tunneling và bảo mật).
* Tích hợp **firewall** hoặc **IDS/IPS** vào biên mạng.

---
