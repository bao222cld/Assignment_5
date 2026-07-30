# Hoạt động 8 – Human Review Checklist

**Phạm vi review:** các artifact hiện có trong `activity-1` đến `activity-6`.

**Nguyên tắc đánh giá:** chỉ ghi nhận là hoàn thành khi artifact cung cấp bằng chứng trực tiếp. `Chưa có thông tin` nghĩa là thư mục/tài liệu hiện có không đủ căn cứ để kết luận; không suy diễn từ các nội dung khác.

| # | Checklist | Trạng thái | Căn cứ review |
|---:|---|---|---|
| 1 | Evidence thu thập trước phân tích đủ 5 loại bắt buộc? | **Hoàn thành** | `activity-1/evidence_summary.md` có: Steps to Reproduce, Session Storage, Network request/response, Console log và Environment; kèm ảnh B, C, C2, D, E. |
| 2 | Mọi hypothesis có evidence ủng hộ và evidence loại trừ, không cảm tính? | **Hoàn thành** | `activity-3/03_root_cause_hypotheses.md` nêu cả hai cột **Evidence ủng hộ** và **Evidence Loại trừ** cho H1–H5. |
| 3 | Xác suất được gán có lý do và theo ngưỡng High/Medium/Low? | **Hoàn thành** | Cùng tài liệu trên định nghĩa ngưỡng High/Medium/Low và giải thích mức xác suất cho từng hypothesis. |
| 4 | Root cause được VERIFY bằng thực nghiệm, có số liệu X/Y? | **Hoàn thành** | `activity-4/04_verification_log_summary.md` ghi số liệu H1 20/20, H2 20/20, H3 3/3, H4 5/5, H5 1/1 và verdict cho từng hypothesis. |
| 5 | AI có bịa evidence hoặc số liệu không (đối chiếu nguồn)? | **Chưa đạt** | `activity-4/TIME_AND_QUALITY_REPORT_activity4_H3_H4_H5.md` tự ghi nhận **2 hallucination**: giả định field `role` và gợi ý các service/cache khi chưa có bằng chứng. Vì đã có sai lệch được ghi nhận, không thể đánh dấu mục này là sạch hoàn toàn. Ngoài ra, các artifact đang mâu thuẫn về field `role`, nên cần đối chiếu lại evidence gốc trước khi chốt. |
| 6 | Severity/Priority phù hợp? | **Hoàn thành** | `activity-2/02_ai_bug_analysis.md` và `activity-5/05_bug_report.md` nhất quán: Severity **High**, Priority **P1**, với lý do liên quan đến lộ PII và ảnh hưởng người dùng đã đăng nhập. |
| 7 | Bug Report có trace về Test Case/Requirement? | **Hoàn thành** | `activity-5/05_bug_report.md` có trace tới `TC-LOGIN-PII-001` và requirement `US2 – Login`. |
| 8 | TSR và Release Note chỉ dùng số liệu thật, đúng audience? | **Chưa có thông tin** | `activity-6/06_test_summary_report.md` chỉ có nội dung “Na thuc hien”; `activity-7` không có Release Note (chỉ có `.gitkeep`). Không có TSR/Release Note đủ nội dung để kiểm tra số liệu hoặc audience. |
| 9 | Đã anonymize toàn bộ PII/token? | **Chưa đạt** | Dù phần F của `activity-1/evidence_summary.md` có bản redacted, các phần trước đó vẫn ghi trực tiếp email `qatest01@gmail.com`, tên `Nguyen Van A` và ID; ảnh evidence cũng chưa có tài liệu xác nhận đã che toàn bộ PII. Không thấy token trong artifact, nhưng không thể kết luận toàn bộ PII/token đã được anonymize. |
| 10 | Prompt đã log và có ít nhất 1 cải tiến? | **Chưa đạt** | Có prompt log tại `activity-2/02_ai_bug_analysis.md`, `activity-3/prompt/03_prompt.md`, `activity-4/prompt/04_prompt_log_H1_H2.md`, `activity-5/prompt/05_prompt_log.md`. Tuy nhiên không có bản prompt sau cải tiến, changelog, hoặc nội dung nêu rõ ít nhất một cải tiến đã thực hiện. |

## Các yêu cầu chưa có đủ thông tin

1. **Mục 8 – TSR và Release Note:** thiếu Test Summary Report có nội dung thực chất và thiếu hoàn toàn Release Note. Vì vậy chưa thể đánh giá tính xác thực số liệu hoặc audience.
2. **Phần “token” của mục 9:** không có token nào được thể hiện trong artifact để đối chiếu. Điều này không chứng minh rằng toàn bộ PII/token đã được ẩn danh.

## Các mục đã hoàn thành

- Mục 1: Evidence trước phân tích.
- Mục 2: Hypothesis có evidence ủng hộ và loại trừ.
- Mục 3: Quy tắc gán xác suất.
- Mục 4: Xác minh root cause bằng thực nghiệm có số liệu.
- Mục 6: Severity/Priority.
- Mục 7: Trace từ Bug Report tới Test Case/Requirement.

## Các điểm cần xử lý trước khi chốt

- Rà lại mâu thuẫn về field `role` giữa evidence và báo cáo metrics; chỉ giữ lại kết luận khớp với evidence gốc.
- Che/loại bỏ PII còn hiển thị trong nội dung và ảnh evidence, hoặc thay bằng dữ liệu đã được redacted.
- Bổ sung TSR hoàn chỉnh và Release Note; mỗi số liệu phải truy vết được về log/test evidence, đồng thời nêu audience của Release Note.
- Lưu ít nhất một vòng cải tiến prompt (ví dụ: prompt phiên bản trước/sau và lý do thay đổi).
