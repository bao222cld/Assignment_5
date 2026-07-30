# Test Summary Report – Session Storage PII Exposure Defect (Draft)

## Thông tin báo cáo

| Trường             | Nội dung                                                                   |
| ------------------ | -------------------------------------------------------------------------- |
| Dự Án              | Booking Appointment System                                                 |
| Môi trường         | Staging                                                                    |
| URL                | https://booking-appointment-system-psi.vercel.app/                         |
| Ngày soạn          | 30/07/2026                                                                 |
| Phạm vi            | Tổng hợp kết quả kiểm thử & xác minh lỗi bảo mật BUG-SEC-001 (US2 – Login) |
| Người soạn báo cáo | Đinh Thị Kiều Na                                                           |

---

## 1. Tổng quan

- Test case liên quan re-run: 35 (module US2-Login)
- Pass: 22 | Fail: 12 | N/A: 1 | Untested: 0
- Pass rate: 62.9% (22/35)
- Bug mới phát hiện: 1 (**BUG-SEC-001** – High/P1)

---

## 2. Kết quả điều tra

- Root cause đã xác định & verify bằng thực nghiệm:
  - **H1 (Confirmed – nguyên nhân chính):** Frontend gọi trực tiếp `sessionStorage.setItem()` lưu nguyên response API, không mapping/filter.
  - **H4 (Confirmed – phụ):** Backend API `/bookings/me` trả dư field (`email`, `bookingCount`) UI không dùng.
- 3 giả thuyết khác (H2 – Zustand/Redux persist, H3 – Cache/Service layer, H5 – redux-persist) đã bị loại trừ bằng thực nghiệm.

---

## 3. Rủi ro

- Ảnh hưởng: toàn bộ authenticated users, tái hiện 100% qua 2 lần đo độc lập:
  - 10/10 lần (nguồn: `02_ai_bug_analysis.md` – Steps to Reproduce, Activity 2 – lần test ban đầu).
  - 20/20 lần (nguồn: `04_verification_log_H1_H2.md` – H1, Activity 4 – con người verify lại bằng chu kỳ Đăng xuất/Đăng nhập).
- Không có workaround khả thi cho end-user (dữ liệu bị lưu ngay khi đăng nhập bình thường); chỉ khắc phục được bằng cách sửa code.
- Compliance risk: vi phạm nguyên tắc bảo mật cơ bản OWASP / lưu trữ PII không mã hóa (liên quan GDPR).

---

## 4. Khuyến nghị

- NO-GO (Không đủ điều kiện để Release lên Production) cho đến khi fix và verify lại.
- Đề xuất fix:
  - Frontend: Sửa logic lưu sessionStorage, loại bỏ việc lưu raw API response. Bắt buộc map/filter dữ liệu chỉ giữ lại thông tin session cần thiết (id, name, role).
  - Backend: Tối ưu hóa API GET /bookings/me, cắt bỏ các field dữ liệu nhạy cảm không phục vụ UI (email, bookingCount).
- Bổ sung test case kiểm tra Session/Local Storage không chứa PII vào regression suite (đặc biệt dạng test US2-Login-27).

---

## 5. Số liệu đo (AI-assisted, Activity 2)

Số liệu đo lường hiệu quả của AI trong khâu Phân tích nguyên nhân gốc rễ (Root Cause Analysis) cho lỗi BUG-SEC-001

- AI Time: 1.5 phút | Human Review Time: 2.0 phút | Manual Est.: 15.0 phút
- Effective time saved: ~11.5 phút/lần phân tích (15.0 − 3.5 phút, tương đương ~77%)
- Hallucination rate: 0/1 (0%) − AI tuân thủ strict rules, không tự đề xuất root cause khi chưa đủ bằng chứng

---

## Tham chiếu

- `02_ai_bug_analysis.md` – Activity 2: AI Bug Analysis
- `04_verification_log_H1_H2.md`, `04_verification_log_H3_H4_H5.md`, `04_verification_log_summary.md` – Activity 4: Human Verification
- `05_bug_report.md` – Bug Report BUG-SEC-001
