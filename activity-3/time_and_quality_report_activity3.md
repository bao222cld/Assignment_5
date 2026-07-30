# TIME_AND_QUALITY_REPORT.md

## Đo lường thời gian & chất lượng – Hoạt động 3: Root Cause Hypotheses (RCA)

### Phạm vi công việc

*   Thiết kế Prompt kỹ thuật (ROOT-CAUSE-001) để phân tích nguyên nhân gốc rễ.
*   Đưa ra ít nhất **05 giả thuyết** kỹ thuật dựa trên bằng chứng (Evidence-based).
*   Đánh giá xác suất (High/Medium/Low) và đề xuất phương pháp kiểm chứng (Verify methods).
*   Sản phẩm: `03_root_cause_hypotheses.md`.

---

## Bảng đo lường

| Hạng mục                                | AI time | Human review | Manual est. | Hallucination | Artifacts |
| :-------------------------------------- | :-----: | :----------: | :---------: | :-----------: | :-------: |
| Phân tích & Sinh 05 giả thuyết (RCA)    | 5 phút  |   15 phút    |   45 phút   |       1       |     5     |
| Hoàn thiện báo cáo                      | 3 phút  |    5 phút    |   15 phút   |       0       |     1     |
| **Tổng**                                | **8 phút** | **20 phút** | **60 phút** |     **1**     |   **6**   |

---

## Giải thích Hallucination

1.  **H4 (Redux-persist):** AI giả định hệ thống có thể dùng Redux-persist do đây là thư viện phổ biến. Tuy nhiên, qua đối soát thực tế với Naming Convention (`auth_state_v1`), Human Review xác định đây là "ảo giác" nhẹ về mặt thư viện (thực tế code nghiêng về Zustand hoặc Custom Logic).
2.  **Thông số giả định:** AI đôi khi giả định sự tồn tại của các Middleware bảo mật phía Backend (như Passport.js) dù chưa có bằng chứng cụ thể trong bản build Staging.

---

# Metric phái sinh

## ROI

ROI = Manual est. / (AI time + Human review)

= 60 / (8 + 20)

= **2.14x**

→ AI giúp tăng tốc quá trình "động não" (brainstorming) và liệt kê giả thuyết nhanh hơn gấp **2.14 lần** so với làm thủ công.

---

## Effective ROI

Ước lượng thời gian sửa hallucination & điều chỉnh xác suất: **5 phút**

Effective ROI = 60 / (8 + 20 + 5)

= **1.81x**

---

## Hallucination Rate

Hallucination Rate = Hallucination / Tổng artifacts

= 1 / 6

= **16.7%**

---

# Nhận xét

*   **Tính đa dạng:** AI hỗ trợ cực tốt trong việc mở rộng các hướng tấn công/nguyên nhân mà Tester có thể bỏ sót (ví dụ: gợi ý về lệch đồng hồ client-server hoặc lỗi cấu hình Load Balancer).
*   **Tư duy phản biện:** Việc ép AI tìm bằng chứng "Loại trừ" (Evidence Against) giúp nhóm hình thành tư duy phân tích sắc bén hơn.
*   **Vai trò con người:** Human review là chốt chặn quan trọng để loại bỏ các giả thuyết "ngoài tầm với" của stack công nghệ thực tế đang sử dụng.

---

# Kết luận

Hoạt động 3 đã hoàn thành và vượt qua **Quality Gate #2**:
*   Đạt số lượng 05 giả thuyết kỹ thuật.
*   Xác suất được gán dựa trên logic đối soát dữ liệu (Evidence-based).
*   Cung cấp lộ trình verify rõ ràng cho đội ngũ **Verification Engineers (Hoạt động 4)**.

Toàn bộ các câu lệnh tối ưu đã được bàn giao cho Người số 6 để đưa vào **Prompt Library**.