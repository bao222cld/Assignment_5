# 03_root_cause_hypotheses.md

## PROMPT_ID: ROOT-CAUSE-001
**Role:** Senior Security QA Engineer & Software Architect
**Mode:** Strict Evidence-Based Analysis (Zero Hallucination / No Emotional Speculation)

---

### Bối cảnh kế thừa (từ Hoạt động 1 & 2)

- Bug: `[US2-Login]` PII (email, họ tên, role) bị lộ dạng plain text trong Session Storage, key `auth_state_v1`.
- API `GET /bookings/me` trả nguyên PII trong response (`id`, `email`, `name`, `role`, `bookingCount`).
- Console log: **No Issues** → hành vi thiết kế, không phải lỗi runtime/crash.
- Stack: Next.js + PostgreSQL, hosted trên Vercel.

> **Giới hạn công cụ (Black-box):** Chỉ có quyền truy cập tab **Sources** (+ Network/Application/Console) trên bản build đã deploy. Không có quyền truy cập backend repo, `package.json`, hay tài liệu nội bộ (SDLC/Confluence). Tab Sources chỉ hiển thị code chạy trên trình duyệt — route handler/DB query của Next.js không bao giờ xuất hiện ở đây.

---

### Quy ước gán xác suất (BẮT BUỘC theo evidence)

| Mức | Điều kiện |
| :--- | :--- |
| **High (>60%)** | ≥ 2 evidence trực tiếp ủng hộ |
| **Medium (30–60%)** | 1 evidence ủng hộ, chưa loại trừ |
| **Low (<30%)** | Chỉ suy luận, chưa có evidence trực tiếp |

*Tổng xác suất KHÔNG bắt buộc = 100% (nguyên nhân có thể độc lập/đồng thời).*
*Lưu ý: mức xác suất ở tầng Architectural/Process (Layer 2) mang tính suy luận gián tiếp nhiều hơn, vì phần lớn không thể quan sát trực tiếp qua DevTools.*

---

## Hoạt động 3 — AI đưa ra Root Cause Hypotheses (≥5)

| Rank | Hypothesis | Prob. | Evidence Ủng hộ | Evidence Loại trừ | Cách verify |
| :-: | :--- | :-: | :--- | :--- | :--- |
| 1 | **Direct Response Mapping / Custom setItem:** một hàm/hook tự viết gọi `sessionStorage.setItem()` ngay sau khi fetch `/bookings/me` thành công, lưu nguyên object response, không qua thư viện state management nào | **High 60%** | (1) Cấu trúc JSON trong storage khớp 100% với response API, kể cả field `bookingCount` không liên quan gì tới "auth"; (2) Field `fetchedAt` là timestamp tuỳ biến, không phải artifact chuẩn của bất kỳ thư viện persist phổ biến nào → gợi ý logic cache tự viết tay | Nếu sau này search Sources tab tìm thấy chuỗi `persist(` hoặc `createJSONStorage` bao quanh vị trí gọi `setItem` → sẽ mâu thuẫn với "thủ công hoàn toàn", cho thấy có thư viện đứng sau (chuyển trọng số sang Rank 2) | Search Sources tab (Ctrl+Shift+F) cho chuỗi literal `sessionStorage.setItem` và `auth_state_v1` — chuỗi này vẫn còn nguyên sau minify.|

| 2 | **State Persistence library (Zustand/Redux persist)** auto-sync toàn bộ "Auth State" vào storage, không có `partialize`/whitelist | **Medium 35%** | Naming `auth_state_v1` (hậu tố `_v<n>`) — lưu ý: đây là weak/indicator evidence, không phải direct evidence. Rất nhiều team tự đặt tên key dạng này mà không hề dùng bất kỳ thư viện persist nào | Cấu trúc thực tế quan sát được là object phẳng, không có wrapper `{state: {...}, version: ...}` đặc trưng của Zustand persist chuẩn | Search Sources tab cho `"zustand"`, `persist(`, `createJSONStorage`, `partialize`. |

| 3 | **Cache/Service abstraction layer** (vd. `AuthService`/`UserService`) mặc định persist toàn bộ API response thay vì mapping sang ViewModel/PersistModel trước khi lưu — *(hypothesis mới, theo đề xuất ChatGPT — "Gap 1")* | **Medium 35%** | Việc `bookingCount` và `fetchedAt` (không liên quan trực tiếp tới "auth") cùng nằm chung 1 object với `user` gợi ý một lớp cache generic áp dụng cho toàn bộ response, không phải logic riêng cho auth | Không phân biệt được rõ ràng với Rank 1 chỉ dựa evidence hiện có — có thể là cùng 1 hiện tượng nhìn từ 2 góc độ (đều thuộc nhóm "thiếu mapping layer"); cần source code để tách bạch | Search Sources tab tìm class/module/hook tên `AuthService`, `UserService`, `useAuthCache`, hoặc pattern `cache(response)` → nếu tìm thấy 1 hàm generic áp dụng cho nhiều loại response (không chỉ auth) |

| 4 | **Backend trả nhiều field hơn mức UI thực sự cần** ở `/bookings/me` (chưa xác định frontend có dùng email/name ở đâu hay không) | **Medium–High 55%** | Response chứa PII dạng plain text (`email`, `name`) thay vì chỉ ID/token tham chiếu — đây vẫn là mắt xích cần thiết trong chuỗi nhân quả dẫn tới lỗi, bất kể "đúng thiết kế" hay không | Nếu UI có hiển thị tên/email ở bất kỳ đâu (welcome banner, header, avatar tooltip...) → API trả các field này là hợp lý theo thiết kế, không phải over-exposure; khi đó bug chuyển hẳn trọng tâm về tầng persist (Rank 1–3), không phải tầng API | (1) Kiểm tra UI thực tế xem có hiển thị email/name ở đâu không — nếu không có chỗ nào hiển thị nhưng API vẫn trả về → bằng chứng mạnh cho "trả thừa"; (2) So sánh với endpoint khác trả info người dùng khác xem pattern lộ PII có nhất quán không; (3) Thử `/api/docs`, `/swagger` — lưu ý: chỉ chứng minh được schema hiện tại, KHÔNG chứng minh được có/không có DTO filter layer, nên đây là verify yếu, chỉ mang tính tham khảo |

| 5 | Dùng **Redux + redux-persist**, storage adapter là `sessionStorage`, thiếu `transform`/`blacklist` | **Low 15%*** | Pattern "persist toàn bộ auth slice" phổ biến trong redux-persist | `redux-persist` chuẩn lưu dưới key `persist:root`/`persist:<reducerName>`, giá trị là chuỗi JSON stringify lồng — khác hẳn object phẳng quan sát được; Application tab chỉ thấy đúng 1 key `auth_state_v1` | Search Sources tab cho `"redux-persist"`, `REHYDRATE`, `persist/PERSIST`, `purgeStoredState`; kết hợp xác nhận lại Application tab không có key `persist:*` |

---

### ▶ Quality Gate #2 — Hypothesis Quality

- [x] ≥ 5 hypotheses 
- [x] Mỗi hypothesis có xác suất + lý do gán
- [x] Có cả evidence ủng hộ và evidence loại trừ thực sự cho từng dòng 
- [x] Không có hypothesis nào chỉ dựa "cảm tính"
