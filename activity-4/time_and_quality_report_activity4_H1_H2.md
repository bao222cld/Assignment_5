# TIME_AND_QUALITY_REPORT_H1_H2.md

## Đo lường thời gian & chất lượng – Phần Human Verification (H1–H2)

### Phạm vi công việc

* Kiểm chứng thực nghiệm các giả thuyết **H1, H2**
* Công cụ sử dụng: Chrome DevTools (Application, Network, Sources, Console)
* Hệ thống kiểm thử: `https://booking-appointment-system-psi.vercel.app/`
* Sản phẩm: `04_verification_log_H1_H2.md` + 4 hình ảnh bằng chứng

---

## Bảng đo lường

| Hạng mục                                       |    AI time | Human review |  Manual est. | Hallucination | Artifacts |
| :--------------------------------------------- | ---------: | -----------: | -----------: | ------------: | --------: |
| H1 – Direct Response Mapping / Custom setItem  |     2 phút |      15 phút |      35 phút |             0 |         3 |
| H2 – State Persistence Library                 |     2 phút |      15 phút |      30 phút |             0 |         2 |
| Viết báo cáo `04_verification_log_H1_H2.md`    |     2 phút |       5 phút |      15 phút |             0 |         1 |
| **Tổng**                                       | **6 phút** |  **35 phút** |  **80 phút** |         **0** |     **6** |

---

## Giải thích Hallucination

* **Hallucination = 0:** Trong phần việc này, AI cung cấp thông tin lý thuyết và các từ khóa tìm kiếm (như `auth_state_v1`, `zustand`, `persist(`) hoàn toàn chính xác với thực tế mã nguồn của dự án, không có lỗi tự bịa đặt thông tin.

---

# Metric phái sinh

## ROI

ROI = Manual est. / (AI time + Human review)

= 80 / (6 + 35)

= **1.95x**

→ AI giúp giảm khoảng **49% thời gian** (tăng tốc độ làm việc gấp **1.95 lần**) so với việc tự rà soát file bundle tĩnh hoàn toàn thủ công.

---

## Effective ROI

Ước lượng thời gian sửa hallucination: **0 phút** (do không phát sinh lỗi hallucination)

Effective ROI = 80 / (6 + 35 + 0)

= **1.95x**

---

## Hallucination Rate

Hallucination Rate = Hallucination / Tổng artifacts

= 0 / 6

= **0%**

---

# Nhận xét

* AI hoạt động rất tốt trong vai trò trợ lý định hướng: cung cấp chính xác các từ khóa tìm kiếm và gợi ý cấu trúc báo cáo nhanh chóng.
* Vai trò của con người (Tester) là quyết định nhất ở bước trực tiếp F12, kiểm chứng trên trình duyệt thực tế và ghi lại kết quả đúng chuẩn.
* Kết hợp AI giúp giảm tối đa thời gian "phỏng đoán mò", tối ưu hóa quy trình kiểm định lỗi.

---

# Kết luận

Phần Human Verification (H1-H2) đã hoàn thành với kết luận:

* **H1: Confirmed** (Xác nhận lỗi do ghi đè thủ công response API)
* **H2: Rejected** (Loại trừ lỗi do cấu hình tự động của thư viện)

Toàn bộ kết luận dựa trên bằng chứng kỹ thuật thực tế thu thập từ môi trường staging.
