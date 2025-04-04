## **Bước 1: Tạo VLAN Interface trên Winbox**
1. Mở Winbox → kết nối đến MikroTik.  
2. Vào **Interfaces** → bấm **+** → chọn **VLAN**.  

   - **VLAN 10**:  
     - *Name*: `vlan10`  
     - *VLAN ID*: `10`  
     - *Interface*: `ether1` (hoặc bridge nếu dùng switch chip)  
     → Bấm **Apply**  

   - **VLAN 20**:  
     - *Name*: `vlan20`  
     - *VLAN ID*: `20`  
     - *Interface*: `ether1`  
     → Bấm **Apply**  


## **Bước 2: Gán IP cho VLAN**
1. Vào **IP → Addresses** → bấm **+**.  
   - **VLAN 10**:  
     - *Address*: `192.168.10.1/24`  
     - *Interface*: `vlan10`  
     → Bấm **Apply**  

   - **VLAN 20**:  
     - *Address*: `192.168.20.1/24`  
     - *Interface*: `vlan20`  
     → Bấm **Apply**  


## **Bước 3: Cấu hình Switch Port (nếu dùng switch chip)**
*Áp dụng cho thiết bị có switch chip (RB750Gr3, hEX, hAP ac2,...)*  

1. Tạo bridge (nếu chưa có):  
   - Vào **Bridge** → bấm **+** → đặt tên (ví dụ: `bridge-local`) → **Apply**.  
   - Vào tab **Ports** → thêm các cổng (`ether2`, `ether3`, `ether4`) vào bridge.  

2. Cấu hình VLAN trên bridge:  
   - Vào tab **VLAN** → bấm **+**:  
     - **VLAN 10**:  
       - *VLAN ID*: `10`  
       - *Bridge*: `bridge-local`  
       - *Tagged*: `ether4` (cổng trunk)  
       - *Untagged*: `ether2` (cổng access VLAN 10)  
       → **Apply**  

     - **VLAN 20**:  
       - *VLAN ID*: `20`  
       - *Bridge*: `bridge-local`  
       - *Tagged*: `ether4`  
       - *Untagged*: `ether3` (cổng access VLAN 20)  
       → **Apply**  


## **Bước 4: Kiểm tra**
1. **Kiểm tra VLAN interface**:  
   - Trên Winbox, vào **Interfaces** → xem `vlan10` và `vlan20` đã active chưa.  
   - Dùng lệnh:  
     ```
     /interface vlan print
     ```  

2. **Kiểm tra IP**:  
   - Ping từ máy thuộc VLAN 10 (`192.168.10.2`) đến gateway (`192.168.10.1`).  
   - Tương tự với VLAN 20.  

3. **Kiểm tra switch port**:  
   - Cổng `ether2` chỉ thuộc VLAN 10, `ether3` chỉ VLAN 20.  
   - Cổng `ether4` (trunk) phải chuyển được cả 2 VLAN.  

---
