# Hoạt động 4 – Human Verification (Thực nghiệm)

> Nguyên tắc: KHÔNG tin AI mù quáng. Phải chạy thực nghiệm để xác nhận/loại trừ.

---

## Môi trường kiểm thử

- Staging URL: https://booking-appointment-system-psi.vercel.app/
- Browser: Chrome 126 / Chrome 150.0.7871.187 (64-bit)
- OS: Windows 11
- Công cụ: Chrome DevTools (Application, Network, Sources, Console)

---

## 1. Bảng Tổng hợp Kết quả Thực nghiệm (Verification Log)

Dưới đây là tập hợp kết quả thực nghiệm thực tế từ các điều tra viên cho 5 giả thuyết (H1 đến H5):

| Hypothesis | Thí nghiệm (Cách verify) | Kết quả thực tế (Số liệu) | Verdict |
| :--- | :--- | :--- | :---: |
| **H1 (Custom setItem)** | Tìm kiếm từ khóa `auth_state_v1` trong tab Sources (Ctrl+Shift+F) để xem logic lưu trữ. | **20/20 lần (100%)** phát hiện code React Context dùng `JSON.stringify` ép kiểu response thô lưu thẳng vào Session Storage, không qua filter. | **Confirmed**<br>*(Chính)* |
| **H2 (Zustand / Redux persist thiếu whitelist)** | Tìm kiếm `zustand`, `persist(`, `partialize` trong mã nguồn. Đánh giá format JSON trong Application tab. | **20/20 lần (0%)** không thấy thư viện; JSON dạng phẳng, thiếu bọc metadata `{state: {...}}` đặc trưng của thư viện persist. | **Rejected** |
| **H3 (Cache / Service layer persist response)** | Tìm kiếm các từ khóa `AuthService`, `cache(`, `useAuthCache` trong mã nguồn. | **3/3 lần** search không ra kết quả (*No matches found*) cho các layer service này. | **Rejected** |
| **H4 (Backend trả dư field so với UI)** | Đối chiếu dữ liệu trả về từ API `/bookings/me` với giao diện UI thực tế của hệ thống. | **5/5 vị trí UI** được kiểm tra chỉ hiển thị Name. Không có chỗ nào cần dùng đến Email hay `bookingCount`, nhưng API vẫn trả về các field này. | **Confirmed**<br>*(Góp phần)* |
| **H5 (Redux + redux-persist storage)** | Kiểm tra key lưu trữ ở Application tab và tìm kiếm chuỗi `redux-persist`, `REHYDRATE` ở tab Sources. | **1/1 lần** không thấy key `persist:root`. Không có dấu vết của Redux persist trong mã nguồn. | **Rejected** |

---

## 2. Kết luận Root Cause (Đã Verify)

Dựa trên bằng chứng thực nghiệm, nguyên nhân gốc rễ gây ra lỗi lộ PII (`email`, `name`, `role`) **không đến từ các thư viện quản lý trạng thái** như AI dự đoán, mà là sự cộng hưởng của hai lỗi thiết kế chủ quan từ cả Backend và Frontend:

### Nguyên nhân được xác nhận (Confirmed):
1. **Lỗi Frontend (Chính - H1):** Lập trình viên tự viết hàm (*Custom hook/Context*) lấy trực tiếp object chứa API response thô và gọi lệnh `sessionStorage.setItem()` lưu thẳng vào trình duyệt dưới key `auth_state_v1`. Quá trình này thiếu hoàn toàn bước mapping dữ liệu hay làm sạch (*data sanitization*) để loại bỏ PII.
2. **Lỗi Backend (Góp phần - H4):** API `GET /bookings/me` được thiết kế over-exposure, trả về dư thừa thông tin cá nhân dạng plain text (`email`, `bookingCount`) trong khi giao diện UI thực tế hoàn toàn không có nhu cầu sử dụng hay hiển thị các trường này.

### Đã loại trừ (Rejected):
- Lỗi không phải do cấu hình sai của các thư viện quản lý trạng thái (*Zustand, Redux-persist*).
- Hệ thống không tồn tại lớp Cache / Service abstraction layer nào gây ra hành vi đồng bộ dư thừa như AI suy đoán (*H2, H3, H5 đều sai*).

---

## Quality Gate #3 – Verification

- [x] Mỗi hypothesis có thí nghiệm cụ thể.
- [x] Có số liệu (X/Y lần fail), không nói chung chung.
- [x] Root cause cuối cùng được verify từ con người, không sử dụng lý do "AI nói vậy".
