## PROMPT_ID: VERIFY-H3-H5-001

**Vai trò:** Chuyên gia QA cấp cao và Kiểm thử bảo mật (Senior QA Engineer & Security Tester)
**Chế độ:** Kiểm chứng Black-box dựa trên bằng chứng thực tế (Evidence-Based Verification)

---

## Hệ thống kiểm thử

* URL: `https://booking-appointment-system-psi.vercel.app/`
* Trình duyệt: Chrome 126
* Hệ điều hành: Windows 11
* Công cụ được phép sử dụng: Chrome DevTools (**Application, Network, Sources, Console**)
* Giới hạn: Không có quyền truy cập source code backend, repository, package.json hoặc tài liệu nội bộ.

---

## Mục tiêu

Thực hiện kiểm chứng thực nghiệm cho các giả thuyết **H3, H4 và H5** trên hệ thống staging đã deploy.
Đối với mỗi giả thuyết, hãy cung cấp:

* Các bước kiểm thử
* Bằng chứng quan sát được
* Kết quả định lượng
* Kết luận (**Confirmed / Rejected**)
* Tên file screenshot cần thu thập

---

# H3 – Cache/Service abstraction layer lưu toàn bộ API response

### Giả thuyết

Một lớp cache/service generic tự động lưu toàn bộ response người dùng vào Session Storage mà không mapping dữ liệu.

### Nhiệm vụ kiểm chứng

1. Mở **Application → Session Storage**.
2. Tìm key `auth_state_v1`.
3. Xóa key này.
4. Reload trang và đăng nhập lại nếu cần.
5. Kiểm tra xem key có được tạo lại tự động hay không.
6. Mở **Network** và kiểm tra request lấy thông tin người dùng.
7. So sánh dữ liệu trong Network Response với Session Storage.
8. Mở **Sources → Search** và tìm các từ khóa:

   * `AuthService`
   * `cache(`
   * `UserService`
   * `useAuthCache`

### Kết quả mong đợi

* Trạng thái tạo lại key sau reload.
* Mức độ trùng khớp giữa storage và API response.
* Kết quả search source code.
* File evidence:

  * `evidence_h3_storage.png`
  * `evidence_h3_network.png`
  * `evidence_h3_sources_search.png`

---

# H4 – Backend trả nhiều dữ liệu hơn UI thực sự cần

### Giả thuyết

Backend trả dư dữ liệu PII mà giao diện không sử dụng.

### Nhiệm vụ kiểm chứng

1. Quan sát giao diện sau khi đăng nhập:

   * Header
   * Avatar menu
   * Booking page
   * Welcome banner
   * Profile page
2. Ghi nhận các trường dữ liệu thực sự được hiển thị.
3. So sánh với dữ liệu trả về từ API.

### Kết quả mong đợi

* Danh sách field hiển thị trên UI.
* Danh sách field trả về từ API.
* File evidence:

  * `evidence_h4_ui_header.png`
  * `evidence_h4_network_response.png`

---

# H5 – Redux Persist

### Giả thuyết

Ứng dụng sử dụng `redux-persist` với `sessionStorage`.

### Nhiệm vụ kiểm chứng

1. Mở **Application → Session Storage**.
2. Ghi nhận toàn bộ key hiện có.
3. Kiểm tra sự tồn tại của `persist:root`.
4. Mở **Sources → Search** và tìm:

   * `redux-persist`
   * `persist:root`
   * `REHYDRATE`
   * `persist/PERSIST`

### Kết quả mong đợi

* Danh sách key storage.
* Dấu hiệu có hoặc không có redux-persist.
* File evidence:

  * `evidence_h5_application_keys.png`
  * `evidence_h5_sources_search.png`

---

# Định dạng báo cáo đầu ra

Đối với mỗi giả thuyết, xuất kết quả theo cấu trúc:

## Giả thuyết

<nội dung giả thuyết>

## Các bước kiểm thử

<liệt kê từng bước>

## Bằng chứng quan sát được

<chỉ ghi nhận sự kiện thực tế, không suy đoán>

## Kết quả định lượng

<ví dụ: 3/3 lần tái hiện>

## Kết luận

Confirmed / Rejected

## File evidence

<danh sách screenshot>

---

# Quy tắc bắt buộc

* Chỉ sử dụng bằng chứng quan sát được từ DevTools.
* Không suy đoán implementation backend.
* Không khẳng định framework/thư viện nếu không có bằng chứng.
* Nếu không quan sát được, ghi rõ **"Không quan sát được"**.
* Kết luận cuối cùng phải dựa trên evidence thực tế đã thu thập.
