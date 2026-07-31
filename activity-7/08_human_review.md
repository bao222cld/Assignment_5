# Hoạt động 8 – Human Review Checklist

## Thông tin báo cáo

| Trường             | Nội dung                                                                   |
| ------------------ | -------------------------------------------------------------------------- |
| Dự Án              | Booking Appointment System                                                 |
| Môi trường         | Staging                                                                    |
| URL                | https://booking-appointment-system-psi.vercel.app/                         |
| Ngày soạn          | 30/07/2026                                                                 |
| Phạm vi            | Các artifact hiện có trong `activity-1` đến `activity-6`.                  |
| Người soạn báo cáo | Vũ Thanh Tùng                                                              |

**Nguyên tắc đánh giá:** chỉ ghi nhận là hoàn thành khi artifact cung cấp bằng chứng trực tiếp. `Chưa có thông tin` nghĩa là thư mục/tài liệu hiện có không đủ căn cứ để kết luận; không suy diễn từ các nội dung khác.

| # | Checklist | Trạng thái | Căn cứ review |
|---:|---|---|---|
| 1 | Evidence thu thập trước phân tích đủ 5 loại bắt buộc? | **Hoàn thành** | `activity-1/evidence_summary.md` có: Steps to Reproduce, Session Storage, Network request/response, Console log và Environment; kèm ảnh B, C, C2, D, E. |
| 2 | Mọi hypothesis có evidence ủng hộ và evidence loại trừ, không cảm tính? | **Hoàn thành** | `activity-3/03_root_cause_hypotheses.md` nêu cả hai cột **Evidence ủng hộ** và **Evidence Loại trừ** cho H1–H5. |
| 3 | Xác suất được gán có lý do và theo ngưỡng High/Medium/Low? | **Hoàn thành** | Cùng tài liệu trên định nghĩa ngưỡng High/Medium/Low và giải thích mức xác suất cho từng hypothesis. |
| 4 | Root cause được VERIFY bằng thực nghiệm, có số liệu X/Y? | **Hoàn thành** | `activity-4/04_verification_log_summary.md` ghi số liệu H1 20/20, H2 20/20, H3 3/3, H4 5/5, H5 1/1 và verdict cho từng hypothesis. |
| 5 | AI có bịa evidence hoặc số liệu không (đối chiếu nguồn)? | **Chưa đạt** | `activity-4/TIME_AND_QUALITY_REPORT_activity4_H3_H4_H5.md` ghi nhận **2 hallucination**: giả định field `role` và nêu service/cache khi chưa có bằng chứng. `activity-6/06_test_summary_report.md` lại ghi Hallucination Rate **0% (0/1)**, nhưng không giải thích hoặc đối chiếu với 2 hallucination trên. Vì số liệu chưa nhất quán, không thể xác nhận mục này đạt. |
| 6 | Severity/Priority phù hợp? | **Hoàn thành** | `activity-2/02_ai_bug_analysis.md` và `activity-5/05_bug_report.md` nhất quán: Severity **High**, Priority **P1**, với lý do liên quan đến lộ PII và ảnh hưởng người dùng đã đăng nhập. |
| 7 | Bug Report có trace về Test Case/Requirement? | **Hoàn thành** | `activity-5/05_bug_report.md` có trace tới `TC-LOGIN-PII-001` và requirement `US2 – Login`. |
| 8 | TSR và Release Note chỉ dùng số liệu thật, đúng audience? | **Chưa đạt** | Đã có `activity-6/06_test_summary_report.md` và `activity-6/07_release_note.md`. Release Note có chỉ dẫn và cách viết hướng đến end-user, nên audience được thể hiện. Tuy nhiên TSR nêu các số liệu 35 test case, 22 Pass, 12 Fail, 1 N/A, các thời gian 5/20/45 phút và Hallucination Rate 0% (0/1), nhưng các tài liệu được liệt kê trong phần tham chiếu của TSR không nêu nguồn truy vết trực tiếp cho toàn bộ các số liệu này; riêng 0% còn mâu thuẫn với 2 hallucination đã ghi ở Activity 4. Release Note là bản nháp cho tương lai nhưng khẳng định “khắc phục hoàn toàn”, “an toàn tuyệt đối” và “chưa ghi nhận bất kỳ vấn đề” mà không có evidence về bản fix hoặc kiểm thử sau fix. |
| 9 | Đã anonymize toàn bộ PII/token? | **Chưa toàn bộ** | Phần F của `activity-1/evidence_summary.md` đã áp dụng đúng REDACTED cho ID/email/tên. Tuy nhiên phần B/C và ảnh evidence trước đó vẫn còn email qatest01@gmail.com, tên Nguyen Van A, User ID 5c8c6dcc-0589-49cc-9847-cd750dcc0ef7 ở dạng RAW. Về token: không phát hiện trong phạm vi đã kiểm tra — không đủ để kết luận đã che token, chỉ có thể nói là không có bằng chứng ngược lại.
có ok không |
| 10 | Prompt đã log và có ít nhất 1 cải tiến? | Đã cải tiến nhưng chưa đủ bằng chứng | Có prompt log tại activity-2/02_ai_bug_analysis.md, activity-3/prompt/03_prompt.md, activity-4/prompt/04_prompt_log_H1_H2.md, activity-5/prompt/05_prompt_log.md và prompt tạo Release Note ở activity-6/07_release_note.md. Nội dung prompt cho thấy đã được bổ sung/ràng buộc nhằm cải thiện chất lượng đầu ra; tuy nhiên chưa có file changelog hoặc bản so sánh trước/sau để nêu rõ thay đổi nào đã được thực hiện và lý do. Cần bổ sung changelog prompt để truy vết quá trình cải tiến. |
## Các yêu cầu chưa có đủ thông tin

1. Nguồn của số liệu TSR (mục 8): Chưa có artifact nguồn cho toàn bộ các số liệu 35 test case, 22 Pass, 12 Fail, 1 N/A và các thời gian 5/20/45 phút. Cần bổ sung log/test run hoặc tham chiếu cụ thể đến nguồn tương ứng.
2. Bằng chứng sau khi fix (mục 8): Chưa có evidence về bản fix hoặc retest sau fix. Vì vậy chưa thể xác thực các khẳng định “khắc phục hoàn toàn”, “an toàn tuyệt đối” và “chưa ghi nhận bất kỳ vấn đề” trong Release Note Draft.
3. Phần “token” của mục 9: Không có token nào được thể hiện trong artifact để đối chiếu. Điều này không chứng minh rằng toàn bộ PII/token đã được ẩn danh.

## Các mục đã hoàn thành

- Mục 1: Evidence trước phân tích.
- Mục 2: Hypothesis có evidence ủng hộ và loại trừ.
- Mục 3: Quy tắc gán xác suất.
- Mục 4: Xác minh root cause bằng thực nghiệm có số liệu.
- Mục 6: Severity/Priority.
- Mục 7: Trace từ Bug Report tới Test Case/Requirement.

## Các điểm cần xử lý trước khi chốt
- Đối chiếu và giải quyết hai mâu thuẫn: field role giữa evidence và báo cáo metrics; Hallucination Rate 0% trong TSR so với 2 hallucination đã được ghi nhận tại Activity 4.
- Che hoặc loại bỏ PII còn hiển thị trong nội dung và ảnh evidence; chỉ sử dụng dữ liệu đã được redacted.
- Bổ sung log/test run hoặc tham chiếu trực tiếp cho từng số liệu trong TSR.
- Chỉ giữ nội dung Release Note đã được xác minh sau fix/retest; không dùng khẳng định tuyệt đối khi chưa có bằng chứng.
- Lưu tối thiểu một vòng cải tiến prompt, gồm bản trước, bản sau và lý do thay đổi.
