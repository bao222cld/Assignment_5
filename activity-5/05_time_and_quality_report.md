# TIME_AND_QUALITY_REPORT.md

# Đo lường thời gian & chất lượng -- Activity 5: AI Bug Report

## Phạm vi công việc

-   Sử dụng AI để tạo Bug Report theo chuẩn IEEE 829 / ISTQB.
-   Tổng hợp kết quả từ Activity 3 (Root Cause Hypotheses) và Activity 4
    (Human Verification).
-   Chuẩn hóa các mục: Bug ID, Severity, Priority, Environment, Steps,
    Actual Result, Expected Result, Root Cause và Suggested Fix.
-   Deliverable: `05_bug_report.md`.

------------------------------------------------------------------------

## Bảng đo lường

  ----------------------------------------------------------------------------
  Bước       AI time   Human review   Manual est.   Hallucination   Artifacts
  ---------- -------- -------------- ------------- --------------- -----------
  Bug Report  6 phút     12 phút        35 phút           0             1

  **Tổng**     **6     **12 phút**    **35 phút**       **0**         **1**
              phút**                                               
  ----------------------------------------------------------------------------

> **Giải thích ước lượng**
>
> -   **AI time:** thời gian AI sinh Bug Report từ prompt.
> -   **Human review:** thời gian kiểm tra tính đúng đắn, chỉnh sửa định
>     dạng và đối chiếu với Verification Log.
> -   **Manual est.:** thời gian ước tính nếu viết Bug Report hoàn toàn
>     thủ công.

------------------------------------------------------------------------

# Metric phái sinh

## ROI

ROI = Manual est. / (AI time + Human review)

= 35 / (6 + 12)

= **1.94x**

------------------------------------------------------------------------

## Effective ROI

Do không phát hiện hallucination trong Bug Report sau khi đã sử dụng kết
quả Human Verification nên không phát sinh thời gian sửa lỗi.

Effective ROI = 35 / (6 + 12)

= **1.94x**

------------------------------------------------------------------------

## Hallucination Rate

Hallucination Rate = Hallucination / Artifacts

= 0 / 1

= **0%**

------------------------------------------------------------------------

# Nhận xét

-   AI giúp chuẩn hóa Bug Report theo đúng cấu trúc IEEE 829 / ISTQB.
-   Human Review vẫn cần thiết để kiểm tra Severity, Priority và Root
    Cause trước khi nộp.
-   Việc sử dụng kết quả đã được Human Verification giúp giảm nguy cơ AI
    đưa ra kết luận sai.

------------------------------------------------------------------------

# Kết luận

Activity 5 hoàn thành với 01 Bug Report được tạo theo chuẩn IEEE 829 /
ISTQB.

AI giúp giảm đáng kể thời gian so với viết báo cáo thủ công, trong khi
Human Review đảm bảo tính chính xác của nội dung trước khi bàn giao.
