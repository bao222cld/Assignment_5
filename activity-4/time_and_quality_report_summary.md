# Đo lường thời gian & chất lượng (TIME AND QUALITY REPORT) - Khâu Tổng hợp

**Người thực hiện:** Lê Tuấn Hùng  
**Phần việc phụ trách:** Hoạt động 4 - Tổng hợp kết quả thực nghiệm từ các thành viên khác và viết kết luận nguyên nhân gốc rễ (Root Cause). *(Không bao gồm công đoạn chạy test thực tế)*.

---

## 1. Bảng đo lường số liệu

Dưới đây là bảng thống kê thời gian và chất lượng cho riêng khâu tổng hợp báo cáo Verification:

| Bước | AI time (phút) | Human review (phút) | Manual est. (phút) | Hallucination (số lần) | Artifacts (số lượng) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Tổng hợp Verification (Hoạt động 4)** | 5 | 10 | 30 | 0 | 1 |
| **Tổng cộng** | **5** | **10** | **30** | **0** | **1** |

**Giải thích số liệu:**
* **AI time (5 phút):** Thời gian sử dụng AI để đọc dữ liệu test thô (do các thành viên khác gửi), gộp vào bảng tóm tắt chuẩn định dạng Markdown và hỗ trợ phác thảo đoạn văn kết luận Root Cause.
* **Human review (10 phút):** Thời gian bạn đọc, rà soát lại văn bản AI sinh ra để đảm bảo tính logic, khớp với kết quả test thực tế của team, và đối chiếu chốt chặn Quality Gate #3 trước khi nghiệm thu báo cáo.
* **Manual est. (30 phút):** Ước tính thời gian nếu bạn phải tự đọc các file log rời rạc của đồng đội, tự kẻ bảng tổng hợp, tự chắt lọc thông tin và tự gõ toàn bộ đoạn phân tích kết luận nguyên nhân gốc rễ bằng tay.
* **Artifacts (1):** Là 1 file báo cáo tổng hợp cuối cùng `04_verification_log.md` do chính bạn tạo ra (không tính các file ảnh bằng chứng vì đó là thành quả của người chạy test).

---

## 2. Các chỉ số phái sinh (Metrics)

Dựa trên bảng số liệu của khâu tổng hợp, tổng thời gian thực tế thực hiện với sự hỗ trợ của AI là **15 phút** (5 phút AI + 10 phút Human). Nếu làm thủ công (Manual), ước tính mất khoảng **30 phút**.

* **ROI (Return on Investment): `100%`**
  * *Công thức:* `(Thời gian Manual - Tổng thời gian AI & Human) / Tổng thời gian AI & Human * 100%`
  * *Tính toán:* `(30 - 15) / 15 * 100 = 100%`
  * *Đánh giá:* Việc dùng AI để định dạng bảng biểu và cấu trúc câu từ cho phần kết luận giúp tiết kiệm một nửa thời gian so với việc tự tổng hợp và gõ báo cáo thủ công.
* **Effective ROI (ROI Hiệu dụng): `2.0x`**
  * *Công thức:* `Thời gian Manual / Tổng thời gian AI & Human`
  * *Tính toán:* `30 / 15 = 2.0`
  * *Đánh giá:* Hiệu suất tổng hợp và viết báo cáo của người phụ trách tăng lên gấp 2 lần.
