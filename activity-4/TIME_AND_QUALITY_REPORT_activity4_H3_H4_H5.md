# TIME_AND_QUALITY_REPORT.md

## Đo lường thời gian & chất lượng – Phần Human Verification (H3–H5)

### Phạm vi công việc

* Kiểm chứng thực nghiệm các giả thuyết **H3, H4, H5**
* Công cụ sử dụng: Chrome DevTools (Application, Network, Sources, Console)
* Hệ thống kiểm thử: `https://booking-appointment-system-psi.vercel.app/`

---

## Bảng đo lường

| Hạng mục                              |     AI time | Human review |  Manual est. | Hallucination | Artifacts |
| ------------------------------------- | ----------: | -----------: | -----------: | ------------: | --------: |
| H3 – Cache/Service abstraction layer  |      3 phút |      15 phút |      30 phút |             1 |         3 |
| H4 – Backend trả dư field             |      3 phút |      15 phút |      30 phút |             0 |         2 |
| H5 – Redux Persist                    |      2 phút |      15 phút |      30 phút |             1 |         2 |
| Viết báo cáo `04_verification_log.md` |      2 phút |       5 phút |      15 phút |             0 |         1 |
| **Tổng**                              | **10 phút** |  **50 phút** | **105 phút** |         **2** |     **8** |

---

## Giải thích Hallucination

1. AI ban đầu giả định response có field `role`, nhưng hệ thống thực tế không có field này.
2. AI gợi ý một số tên service/cache (`AuthService`, `UserService`) dù chưa có bằng chứng tồn tại trong bản build staging.

Các giả định trên đã được Human Review kiểm tra và điều chỉnh trước khi hoàn thiện báo cáo.

---

# Metric phái sinh

## ROI

ROI = Manual est. / (AI time + Human review)

= 105 / (10 + 50)

= **1.75x**

→ AI giúp giảm khoảng **43% thời gian** so với làm hoàn toàn thủ công.

---

## Effective ROI

Ước lượng thời gian sửa hallucination: **5 phút**

Effective ROI = 105 / (10 + 50 + 5)

= **1.62x**

---

## Hallucination Rate

Hallucination Rate = Hallucination / Tổng artifacts

= 2 / 8

= **25%**

---

# Nhận xét

* AI hỗ trợ hiệu quả ở bước tạo checklist kiểm chứng và hướng dẫn thu thập evidence.
* Human review là bước bắt buộc để xác nhận kết quả thực nghiệm trên hệ thống thật.
* Giá trị lớn nhất của AI trong phần Human Verification là **tăng tốc quy trình kiểm thử và chuẩn hóa báo cáo**, không thay thế việc xác nhận bằng thực nghiệm.

---

# Kết luận

Phần Human Verification đã hoàn thành với:

* **H3: Rejected**
* **H4: Confirmed**
* **H5: Rejected**

Toàn bộ kết luận được đưa ra dựa trên evidence thực tế thu thập từ môi trường staging bằng Chrome DevTools.
