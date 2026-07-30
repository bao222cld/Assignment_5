# 04_verification_log_H1_H2.md

## Human Verification – Activity 4 (Lê Quốc Anh)

### Môi trường kiểm thử

* **Staging URL:** [https://booking-appointment-system-psi.vercel.app/](https://booking-appointment-system-psi.vercel.app/)
* **Browser:** Chrome 126
* **OS:** Windows 11
* **Công cụ:** Chrome DevTools (Application, Network, Sources, Console)

---

## H1 – Direct Response Mapping / Custom setItem

**Hypothesis:** Một hàm hoặc hook tự viết gọi trực tiếp `sessionStorage.setItem()` ngay sau khi fetch `/bookings/me` thành công, lưu nguyên object response mà không qua mapping hay thư viện quản lý state nào.

### Kết quả thực tế

* Đăng nhập thành công và kiểm tra tab **Application** ➔ **Session Storage** ➔ `https://booking-appointment-system-psi.vercel.app`.
* Phát hiện key `auth_state_v1` được khởi tạo và lưu dưới dạng một JSON object phẳng, chứa đầy đủ thông tin PII nhạy cảm:
  `{"id": "...", "email": "test_qa@example.com", "name": "QA Test", "role": "user", "bookingCount": 0, "fetchedAt": 1785397...}`
* Thực hiện tìm kiếm toàn cục (Ctrl+Shift+F) trong tab **Sources** cho từ khóa `auth_state_v1`, phát hiện code nguồn React Context trong các chunk tĩnh của Next.js (cụ thể là `layout-1c26927149c7d323.js` và `page-c1181ef88698cfce.js`):
  ```javascript
  let i=(0,s.createContext)(null), o="auth_state_v1"; 
  function l(e){
      sessionStorage.setItem(o, JSON.stringify(e))
  }
  ```
  Hàm `l` lấy trực tiếp tham số `e` (API response object thô) và ép kiểu chuỗi bằng `JSON.stringify` để lưu thẳng vào Session Storage, không hề có bất kỳ mapping layer nào nhằm lọc bỏ các trường PII dư thừa (`email`, `bookingCount`).

### Bằng chứng thu thập

* `evidence_h1_session_storage.png`
* `evidence_h1_sources_search.png`

### Số liệu

* Thực hiện chu kỳ Đăng xuất ➔ Đăng nhập lại 20 lần / 20 lần đều xuất hiện chính xác hành vi này (Tỷ lệ tái hiện 100%).

### Verdict

**Confirmed**

---

## H2 – State Persistence library (Zustand/Redux persist) không whitelist

**Hypothesis:** Ứng dụng sử dụng một thư viện quản lý trạng thái tự động đồng bộ (như Zustand hoặc Redux Persist) để lưu toàn bộ "Auth State" mà không có bộ lọc whitelist/partialize.

### Kết quả thực tế

* Thực hiện tìm kiếm trong tab **Sources** cho các từ khóa đặc trưng của thư viện: `zustand`, `persist(`, `createJSONStorage`, `partialize`.
* Kết quả tìm kiếm: Hoàn toàn không phát hiện bất kỳ dấu vết hoặc code của thư viện Zustand/Redux Persist nào được import hay sử dụng trong project để persist state.
* Định dạng dữ liệu thực tế lưu dưới key `auth_state_v1` là dạng phẳng, hoàn toàn thiếu vắng các trường bọc metadata mặc định của Zustand persist dạng `{state: {...}, version: ...}`.

### Bằng chứng thu thập

* `evidence_h2_sources_no_match.png`
* `evidence_h2_storage_format.png`

### Số liệu

* Thực hiện chạy thử nghiệm đăng nhập 20 lần / 20 lần định dạng lưu trữ đều là dạng phẳng, không xuất hiện metadata bọc của thư viện (Tỷ lệ 0%).

### Verdict

**Rejected**
