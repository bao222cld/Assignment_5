# 02_ai_bug_analysis.md

## 1. PROMPT DÃ SỬ DỤNG (PROMPT LOG)

```text
## PROMPT_ID: BUG-ANALYSIS-001
You are a Senior QA Engineer specializing in web app debugging and technical root-cause analysis.
Analyze the bug ONLY using the evidence provided below.

## RULES
1. Do NOT speculate or guess missing information. If evidence is insufficient to conclude any point, explicitly state "Insufficient evidence".
2. Do NOT propose root causes yet (that will be the next step in Activity 3).
3. Classify the bug into standard QA technical categories:
   - Category (e.g., Logic, Session-Auth, Database, UI/UX, Security)
   - Layer affected (Frontend, Backend, Infrastructure, Network)
   - Bug Type (e.g., Intermittent, Race condition, Security Vulnerability)
   - Proposed Severity & Priority with clear technical rationale based on user impact.
4. Summarize a chronological Failure Timeline extracted directly from the provided logs and timestamps.

## EVIDENCE DATA
Bug: [US2-Login] PII (email, họ tên, role) bị lộ dạng plain text trong Session Storage
A. Steps to Reproduce (reproduce rate: 10/10 lần — luôn xảy ra) 
1. Mở Chrome (Incognito), vào https://booking-appointment-system-psi.vercel.app/auth/register
2. Đăng ký tài khoản mới:
   - Họ tên: Nguyen Van A
   - Email: qatest01@gmail.com
   - Password: Test@123
3. Đăng nhập bằng tài khoản vừa tạo
4. Mở F12 → Application → Session Storage → domain của web
5. → Thấy ngay key "auth_state_v1" chứa toàn bộ PII dạng plain text, không mã hóa

B. Session Storage Evidence 
Key: auth_state_v1
Value:
{
  "user": {
    "id": "«REDACTED_ID»",
    "email": "«REDACTED_EMAIL»",
    "name": "«REDACTED_NAME»",
    "role": "PATIENT"
  },
  "bookingCount": 0,
  "fetchedAt": 1785311090806
}

C. Network Request/Response (nguồn gốc dữ liệu) 
GET /bookings/me HTTP/1.1
Host: booking-appointment-system-psi.vercel.app
Status: 200 OK

Response: 
{
  "user": {
    "id": "«REDACTED_ID»",
    "email": "«REDACTED_EMAIL»",
    "name": "«REDACTED_NAME»",
    "role": "PATIENT"
  },
  "bookingCount": 0
}

D. Browser Console Log
Console trống, "No Issues" — không có error/warning nào khi load trang và session được lưu.

E. Môi trường 
App: Booking Appointment System (Next.js + PostgreSQL), staging - Vercel
URL: https://booking-appointment-system-psi.vercel.app
Browser: Google Chrome 150.0.7871.187 (64-bit)
OS: Windows 11, Version 25H2 (Build 26200.8875)
Thời gian test: 29/07/2026
```

---

## 2. KẾT QUẢ AI PHÂN TÍCH (AI OUTPUT)

### 2.1. Bug Classification Table

| Chiều phân loại | Kết quả phân tích |
| :--- | :--- |
| **Category** | Security / Data Privacy & Storage |
| **Layer** | Client-Side Storage (Frontend) + API Response Design (Backend) |
| **Bug Type** | Security Vulnerability / Unencrypted PII Exposure (Deterministic - 100% repro) |
| **Proposed Severity** | **High / Major** |
| **Proposed Priority** | **P1 / High** |

#### Rationale for Severity & Priority:
* **Severity (High):** Lưu trữ PII (Email, Họ tên) dạng plain text dưới `SessionStorage` tạo rủi ro rò rỉ thông tin qua các cuộc tấn công XSS, tiện ích mở rộng trình duyệt (browser extensions) hoặc truy cập vật lý. Điều này vi phạm nguyên tắc bảo mật cơ bản (OWASP / GDPR compliance).
* **Priority (P1):** Tỷ lệ tái lặp **10/10 lần (100%)**, xuất hiện ngay luồng Đăng ký/Đăng nhập cơ bản (`US2-Login`). Rủi ro tuân thủ bảo mật cần được khắc phục sớm trước khi đưa lên môi trường Production.

### 2.2. Failure Timeline (Chronological Order)

- `[Step 1]`: Người dùng truy cập trang Đăng ký (`/auth/register`) trên Google Chrome (Incognito).
- `[Step 2]`: Người dùng thực hiện đăng ký tài khoản mới và đăng nhập thành công vào hệ thống.
- `[Step 3]`: Frontend gửi yêu cầu `GET /bookings/me` tới server (`booking-appointment-system-psi.vercel.app`).
- `[Step 4]`: Server phản hồi `HTTP/1.1 200 OK` chứa thông tin người dùng (`id`, `email`, `name`, `role`, `bookingCount`) dưới dạng plain text JSON.
- `[Step 5]`: Frontend lưu trữ nguyên bản (raw response) dữ liệu người dùng vào `SessionStorage` dưới key `auth_state_v1` mà không qua bước mã hóa/lọc dữ liệu nhạy cảm.
- `[Step 6]`: Mở DevTools (F12) `Application -> Session Storage`, xác nhận key `auth_state_v1` hiển thị đầy đủ PII dạng plain text. Console trống (`No Issues`), xác nhận đây là hành vi luồng dữ liệu hiện tại của ứng dụng.

---

## 3. THÔNG SỐ ĐO LƯỜNG & REVIEW (METRICS LOG FOR HĐ 8)

Để phục vụ việc tổng hợp báo cáo đo lường chất lượng tại **Hoạt động 8 (`TIME_AND_QUALITY_REPORT.md`)**, các thông số thực thi của bước Bug Analysis này được ghi nhận như sau:

* **Bước:** Bug Analysis (Hoạt động 2)
* **AI Time (Thời gian AI tạo kết quả):** 1.5 phút
* **Human Review Time (Thời gian con người kiểm tra):** 2.0 phút
* **Manual Est. (Thời gian ước tính làm thủ công):** 15.0 phút
* **Hallucination Count (Số lỗi AI bịa/sai thực tế):** 0 lỗi (AI tuân thủ strict rules, không đoán Root Cause)
* **Artifacts Count (Số lượng file đầu ra):** 1 file (`02_ai_bug_analysis.md`)
