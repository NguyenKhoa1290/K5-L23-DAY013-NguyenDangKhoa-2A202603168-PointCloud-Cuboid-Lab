# Báo cáo Day 13 — NguyenDangKhoa-2A202603168

> **Cách dùng mẫu này:** đổi tên file thành `<MSSV>_<HoTen>.md` (họ tên không dấu, viết liền), ví dụ `2A202600123_NguyenVanAn.md`. Đổi tên, đừng chép thành file thứ hai.
>
> Xoá mọi dấu TODO và dòng gợi ý (dòng bắt đầu bằng `>`). Mỗi con số lấy từ **bảng A** hoặc **phiếu tự kiểm** trong `submission/`, kèm đơn vị (m, °). Ảnh để trong `report/individual/images/` (nộp LMS trong file zip, không commit); trong báo cáo chỉ ghi **tên file ảnh**. Báo cáo này nằm trên repo public: không chép biển số, chữ trên biển hiệu hay biển chỉ đường, tên đường, địa danh; nhắc tới bạn khác thì ghi MSSV, không ghi họ tên. Cả báo cáo ~2 trang.

## 1. Kết luận trước
- **MSSV:** `2A202603168`. **Task CVAT:** `D13-2A202603168` (task `36`).
- **Hệ điều hành:** Windows. **Phiên bản máy chủ CVAT:** `2.74.1`.
- **10 frame:** `s01f17`, `s01f40`, `s02f12`, `s02f31`, `s03f20`, `s03f54`, `s04f18`, `s04f40`, `s05f15`, `s05f35`.
- **Trình duyệt:** Chrome. **MSSV bạn ngồi cạnh:** 2A202603161

Task có 132 cuboid. Hai frame có ít cuboid nhất là `s01f17` và `s05f35`, mỗi frame có 8 cuboid. Lần tự kiểm gần nhất ghi nhận 10/132 cuboid có cảnh báo.

- Hai frame trên có ít cuboid nhất do mặt đường đã chắn một nửa làn lidar nên ko quét sang bên kia đường được + đường quốc lộ

cách dùng CVAT, 3 dòng: 
Đổi góc nhìn: Xoay : Bấm chuột trái
                zoom ra zoom vào: Chuột phải
                đặt vật thể: Double click để đặt vật thể sau khi chọn cuboid

## 2. Đọc frame (Bước 2)
Hai frame đọc kỹ. Ảnh: `pointcloud/s01f17.pcd` (ảnh nhìn từ trên xuống, có đánh dấu phía trước, +x, +y).

| Frame | Phía trước xe ở cạnh nào của ảnh nhìn từ trên xuống; +x, +y chỉ về đâu | Mặt đường (phẳng/dốc; thấp hơn LiDAR ~ m) | Vật xa nhất đọc được (~ m) | Loại vật thấy | Vật phía sau / hai bên | Cụm chưa đọc được |
|---|---|---|---|---|---|---|
| pointcloud/s01f17.pcd |Trái| +x chỉ về bên phải, +y chỉ lên trên(Hình top)|mặt đường phẳng|Xe ô tô, xe bus|Vật phía sau và hai bên nhìn rõ|Cụm phía bên trái làn đối diện|

chỉ camera thấy:
+ Loại phương tiện
+ Số lượng người, người đang dắt hay đang ngồi trên xe
+ Phương tiện ở xa
Chỉ lidar thấy:
+ Phương tiện làn đối diện
+ Người đi bộ bị vật thể che
+ Xe đạp ở làn riêng



## 3. Ba cuboid có hướng dẫn (Bước 3)
Ảnh: `t2-a-car`, `t2-b-bus-truck`, `t2-c-ped-cyclist`.
pointcloud/s01f17.pcd
| Vật | Frame, idx | Class | Mũi tên yaw đúng hướng? | Đáy chạm đường? | Đã kiểm Top/Side/Front + ảnh? | Kích thước trong bảng 4.3? |
|---|---|---|---|---|---|---|
| (a) xe con gần | pointcloud/s01f40.pcd,17 | car | đúng | đúng | đã kiểm |3,8–5,0 × 1,70–1,90 × 1,40–1,60 m |
| (b) xe buýt hoặc xe tải | pointcloud/s01f17.pcd,8 | bus | đúng | đã sửa | đã kiểm|10,0–12,5 × 2,50–2,60 × 3,00–3,50 m |
| (c) người đi bộ hoặc xe hai bánh | pointcloud/s01f40.pcd,10| cyclist|đúng|đúng|đúng|1,8–2,3 × 0,70–0,95 × 1,40–1,80 m |

Mình đánh sai 2 chỗ, xe bus đánh dưới đường, đánh thiếu 1 cyclist và một box bị lơ lửng, đã thống nhất và sửa

## 4. Bốn hiện tượng chất lượng (Bước 4)
Nguyên nhân ghi là **giả thuyết**. Frame mượn thì ghi rõ nguồn: task chung, hoặc MSSV của bạn cho mượn.

| Hiện tượng | Frame / tên ảnh / nguồn | Nguyên nhân (giả thuyết) | Thấy gì trên point cloud | Hệ quả khi gán (theo guideline) |
|---|---|---|---|---|
| 1. Bóng che | TODO(học viên) | | | |
| 2. Thưa ở xa | pointcloud/s01f40.pcd | Cách một dải phân cách mặt đường|Thấy khuôn xe trên point cloud| Gán được nhưng ko thấy vật|
| 3. Phản xạ thấp / ít điểm bất thường | TODO(học viên) | | | |
| 4. Điểm lạ | TODO(học viên) | | | |

## 5. Số từ cuboid của tôi (Bước 5)
Số đo lấy từ `submission/D13-2A202603168-bangA.csv`. Số điểm và độ hở so với mặt đường ước tính quanh hộp lấy từ `submission/D13-2A202603168-tukiem.csv`. Mốc −1,7 m là độ cao mặt đường tham chiếu.

| Cuboid (frame, idx, class) | z | h | z đáy = z − h/2 | dist_m | r = √(x²+y²+z²) | yaw_deg | Nhận xét (lơ lửng / chìm / chạm đường) |
|---|---|---|---|---|---|---|---|
| `s03f54`, `idx 2`, `Car` | −0,948 m | 1,545 m | −1,720 m | 9,50 m | 9,55 m | 268,9° | Đáy gần mặt đường ước tính, cao hơn 0,07 m. |
| `s01f17`, `idx 7`, `Bus` | −0,040 m | 3,878 m | −1,979 m | 30,68 m | 30,68 m | 270,3° | Đáy cao hơn mặt đường ước tính quanh hộp 0,16 m. |
| `s01f40`, `idx 9`, `Cyclist` | −0,432 m | 1,406 m | −1,135 m | 21,07 m | 21,08 m | 281,2° | Đáy cao hơn mặt đường ước tính 0,38 m, có thể còn lơ lửng. Cần xem lại ở Side/Front. |

- **Khoảng cách tới tâm hộp.** Gần nhất là `Car` (`s01f17`, `idx 8`), `dist_m` = 0,48 m và `r` = 0,88 m. Tâm rất gần LiDAR, nên cần đối chiếu trên CVAT để xác nhận đó có phải xe thu dữ liệu hay không. Xa nhất là `Car` (`s05f15`, `idx 11`), `dist_m` = 54,66 m và `r` = 54,67 m, vượt phạm vi 50 m của bài. Ba hộp khác cũng vượt 50 m theo `dist_m`: `s02f31`/`8` (52,20 m), `s04f18`/`20` (52,32 m) và `s05f35`/`8` (50,29 m).
- **Kích thước dài × rộng × cao.** `Car` (`s03f54`, `idx 2`) đo được **4,443 × 1,850 × 1,545 m**, nằm trong khoảng tham chiếu sedan/hatchback. `Cyclist` (`s01f40`, `idx 9`) đo được **2,026 × 0,820 × 1,406 m**, nằm trong khoảng tham chiếu xe máy có người lái. Các đối chiếu này dùng khoảng kích thước theo loại xe tương ứng trong bảng 4.3.
- **Hướng đầu xe.** Hai hộp `Car` nằm phía trước LiDAR (`y < 0`): `s03f54`/`2` có yaw **268,9°**, gần **270°** và hướng theo chiều trước của xe thu dữ liệu. `s02f12`/`6` có yaw **89,9°**, gần **90°** và hướng ngược lại.

Mật độ theo khoảng cách (4 cuboid ở các `dist_m` khác nhau):

| Cuboid (frame, idx, class) | dist_m | Số điểm (`points_in`) |
|---|---|---|
| `s04f40`, `idx 1`, `Car` | 7,46 m | 1.856 |
| `s01f17`, `idx 3`, `Car` | 14,64 m | 729 |
| `s04f18`, `idx 18`, `Car` | 22,89 m | 227 |
| `s04f18`, `idx 23`, `Car` | 32,05 m | 102 |

Trong bốn mẫu `Car` này, khi khoảng cách tăng từ 7,46 m lên 32,05 m (khoảng 4,3 lần), số điểm trong hộp giảm từ 1.856 xuống 102 (còn khoảng 5,5%). Xu hướng giảm giống số liệu tham chiếu trong bài giảng: khoảng 1.968, 718, 214 và 72 điểm lần lượt ở các dải 0–10, 10–20, 20–30 và 30–40 m.

**Chỉnh sửa sau tự kiểm**

- `Bus`, frame `s01f17` (frame 0), `idx 7`: tăng chiều cao hộp, rồi tăng thêm một chút ở lượt chỉnh sau. Chiều cao hiện tại là **3,878 m**.
- `Cyclist`, frame `s01f40` (frame 1), `idx 9`: tăng chiều cao hộp. Chiều cao hiện tại là **1,406 m**.

Hai hộp trên không có cờ trong phiếu tự kiểm hiện tại. Phiếu ghi nhận **10/132 cuboid có cảnh báo**. Các hộp cần đối chiếu trên CVAT gồm:

- Đáy cao hơn mặt đường ước tính: `s03f20`/`16`, `s04f40`/`10`, `s05f15`/`7`, `s05f35`/`6`.
- Chiều dài: `s03f20`/`18` và `s03f20`/`24`. Hộp `s03f20`/`24` còn có chiều dài nhỏ hơn chiều rộng.
- Chiều rộng: `s03f54`/`8`, `s03f54`/`9`, `s05f15`/`12` và `s05f35`/`5`. Hộp `s05f35`/`5` còn bị cảnh báo về chiều cao.

Giữ lại vì sai số nhỏ so với mặt đường do cảm biến lidar tạo ra

Bạn ngồi cạnh:2A202603161:Cùng kiểm pointcloud/s02f12.pcd, sự khác biệt về bạn kiểm chéo có đánh người ở dải phân cách, tuy nhiên mình không thấy nên mâu thuẫn không gán

## 6. Ý nghĩa của nhãn, cách dùng AI và điều còn chưa chắc
**a. Ý nghĩa của nhãn.** Mô hình học vị trí và kích thước thật của vật trong hệ tọa độ LiDAR, nên các số đo dùng mét; kích thước tính bằng pixel thay đổi theo khoảng cách và góc nhìn camera. Vật thưa điểm cần quy tắc riêng vì khi thiếu căn cứ để xác định hình dạng hoặc kích thước, một hộp vẽ theo phỏng đoán có thể cung cấp nhãn sai cho mô hình.

**b. Cách dùng AI**

- **Công cụ:** ChatGPT (Codex).
- **Câu hỏi chính:** Cách đọc hệ tọa độ VF, dùng `sync`, xem ảnh chiếu và đối chiếu kết quả tự kiểm.
- **Cách dùng kết quả:**  Dùng hướng dẫn để thao tác trên CVAT. Codex hỗ trợ chạy các lệnh của lab, đối chiếu hai CSV đã xuất và soạn một phần báo cáo.

**c. Điều còn chưa chắc.** Khi kiểm chéo frame `s02f12`, bạn bên cạnh gán nhãn một người ở dải phân cách, chưa thấy đủ căn cứ để xác nhận đó là người nên chưa gán nhãn cho trường hợp này.
