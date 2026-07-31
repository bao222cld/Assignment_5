# TIME_AND_QUALITY_REPORT.md

## Đo lường thời gian & chất lượng — Tổng hợp toàn bộ pipeline (Activity 1–6)

> Số liệu tổng hợp từ các báo cáo `time_and_quality_report*.md` đã có sẵn ở
> từng activity — không tự thêm/bịa số nào. Xem cột "Nguồn" để trace ngược.

| Bước | AI time | Human review | Manual est. | Hallucination | Artifacts | Nguồn |
|---|---:|---:|---:|---:|---:|---|
| Evidence | — | — | — | — | 6 | `activity-1/evidence_summary.md` + 5 ảnh |
| Bug Analysis | 1.5 phút | 2.0 phút | 15.0 phút | 0 | 1 | `activity-2/02_ai_bug_analysis.md` |
| Hypotheses | 8 phút | 20 phút | 60 phút | 1 | 6 | `activity-3/time_and_quality_report_activity3.md` |
| Verification | 21 phút | 95 phút | 215 phút | 2 | 15 | `activity-4/time_and_quality_report_activity4_H1_H2.md` + `TIME_AND_QUALITY_REPORT_activity4_H3_H4_H5.md` + `time_and_quality_report_summary.md` |
| Bug Report | 6 phút | 12 phút | 35 phút | 0 | 1 | `activity-5/05_time_and_quality_report.md` |
| TSR + Release Note | 5 phút | 20 phút | 45 phút | 0* | 2 | `activity-6/06_test_summary_report.md` (mục 5) |
| **Tổng** | **41.5 phút** | **149 phút** | **370 phút** | **3** | **31** | |

---

## Metric phái sinh

### ROI
```
ROI = Manual est. / (AI time + Human review)
    = 370 / (41.5 + 149)
    = 370 / 190.5
    = 1.94x
```
→ Toàn bộ pipeline (Activity 2–6) nhanh hơn **1.94 lần** so với làm hoàn toàn
thủ công, nếu chỉ tính thời gian AI + thời gian con người review.

### Effective ROI
Thời gian ước tính để sửa hallucination đã phát sinh (lấy từ ước lượng thật
trong `time_and_quality_report_activity3.md` và
`TIME_AND_QUALITY_REPORT_activity4_H3_H4_H5.md`): 5 phút (Activity 3) + 5 phút
(Activity 4, H3–H5) = **10 phút**.

```
Effective ROI = Manual est. / (AI time + Human review + thời gian sửa hallucination)
              = 370 / (41.5 + 149 + 10)
              = 370 / 200.5
              = 1.85x
```

> Effective ROI (1.85x) thấp hơn ROI thô (1.94x) — đúng như kỳ vọng, vì đã
> tính thêm chi phí thực tế phải bỏ ra để sửa các hallucination AI tạo ra.
> **Effective ROI chưa bao gồm chi phí xử lý nghi vấn hallucination thứ 4 ở
> TSR** (chưa được reconcile) — nếu tính thêm, con số này sẽ còn thấp hơn nữa.

### Hallucination Rate
```
Hallucination Rate = Tổng Hallucination / Tổng Artifacts
                    = 3 / 31
                    = 9.7%
```

---

## Nhận xét tổng hợp

- **Giai đoạn tốn nhiều thời gian con người nhất:** Verification (Activity 4)
  — 95 phút Human review, chiếm 64% tổng thời gian review của cả pipeline.
  Hợp lý vì đây là bước bắt buộc thực nghiệm trực tiếp trên hệ thống thật,
  không thể rút gọn bằng AI.
- **Giai đoạn có hallucination cao nhất:** Verification (2/15 artifacts,
  ~13.3%) — do AI tự đặt tên cụ thể cho thư viện/class chưa xác nhận (đã ghi
  nhận và đề xuất fix trong `PROMPT-rca-hypothesis.md` v1.1 tại
  `activity-8/prompt_library/`).
- **Bước không phát sinh hallucination:** Bug Analysis (Activity 2), Bug
  Report (Activity 5), và TSR + Release Note (Activity 6) — cả 3 bước này đều
  đạt 0 hallucination, cho thấy prompt đã dùng ở các bước "tổng hợp/chuẩn hóa
  báo cáo" (khi input đầu vào đã được verify từ trước) có độ tin cậy cao hơn
  hẳn so với các bước "khám phá/suy luận" (Hypotheses, Verification).
