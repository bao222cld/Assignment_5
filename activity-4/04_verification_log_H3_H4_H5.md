# 04_verification_log.md

## Human Verification – Activity 4

### Môi trường kiểm thử

* Browser: Chrome 126
* OS: Windows 11
* Environment: Staging build đã deploy
* Công cụ: Chrome DevTools (Application, Network, Sources, Console)

---

## H3 – Cache/Service abstraction layer persist toàn bộ API response

**Hypothesis:** Một lớp cache/service generic lưu nguyên response người dùng vào Session Storage mà không mapping dữ liệu.

### Kết quả thực tế

* Sau đăng nhập, key `auth_state_v1` được tạo lại ngay lập tức.
* Giá trị trong storage chứa:

  * `user.id`
  * `user.email`
  * `user.name`
  * `bookingCount`
  * `fetchedAt`
  
* Search trong Sources không tìm thấy chuỗi `cache(`, `AuthService`, `UserService`, `useAuthCache`.

### Bằng chứng thu thập

* `evidence_h3_storage.png`
* `evidence_h3_network.png`
* `evidence_h3_sources_search.png`

### Số liệu

* Chạy 3 lần / 3 lần đều tái hiện cùng kết quả.

### Verdict

**Rejected**

---

## H4 – Backend trả nhiều field hơn UI thực sự cần

**Hypothesis:** API trả dư dữ liệu PII so với nhu cầu hiển thị của UI.

### Kết quả thực tế

* UI chỉ hiển thị tên người dùng.
* Không có màn hình nào hiển thị:

  * email
  * bookingCount
* Dữ liệu email và bookingCount vẫn tồn tại trong storage/API response.

### Bằng chứng thu thập

* `evidence_h4_ui_header.png`
* `evidence_h4_network_response.png`

### Số liệu

* Quan sát 5 vị trí UI / 5 vị trí không hiển thị email và bookingCount.

### Verdict

**Confirmed**

---

## H5 – Redux + redux-persist lưu auth state vào sessionStorage

**Hypothesis:** Ứng dụng dùng redux-persist với storage adapter là sessionStorage.

### Kết quả thực tế

* Chỉ thấy key `auth_state_v1`.
* Không có key `persist:root`.
* Search trong Sources không tìm thấy các chuỗi đặc trưng của redux-persist.

### Bằng chứng thu thập

* `evidence_h5_application_keys.png`
* `evidence_h5_sources_search.png`

### Số liệu

* 1/1 lần kiểm tra không phát hiện dấu hiệu redux-persist.

### Verdict

**Rejected**

---

# Kết luận sau thực nghiệm

| Hypothesis                                            | Kết quả   |
| ----------------------------------------------------- | --------- |
| H3 – Cache/service abstraction layer persist response | Rejected  |
| H4 – Backend trả dư field                             | Confirmed |
| H5 – Redux + redux-persist                            | Rejected  |

## Root Cause cập nhật

* **Nguyên nhân được xác nhận:** Dữ liệu người dùng (email, name, bookingCount) được lưu trong Session Storage dưới key `auth_state_v1`, trong khi UI không sử dụng email và bookingCount.
* **Nguyên nhân bị loại trừ:** Không có bằng chứng về cache/service abstraction layer generic và không có dấu hiệu sử dụng redux-persist trong bản build kiểm thử.
