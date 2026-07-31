# 1. Prompt đã sử dụng
```text
You are a QA Lead drafting a Release Note for end-users.

## CRITICAL RULES
- MUST BE WRITTEN AS A DRAFT FOR THE FUTURE ("sau khi fix").
- Release Note = user-friendly, no jargon, no internal details.
- Do NOT mention any internal testing metrics, NO-GO decisions, or bug IDs.
- Format constraint: Force the Release Note to strictly follow this exact structure:
  ## Release version: 
	(blank for now)
  ## Bug Fixes: 
	(Limit to 1-2 user-friendly sentences)
  ## Cải tiến: 
	(Limit to 1-2 user-friendly sentences)
  ## Known Issues: 
	(Limit to 1 short sentence)

## INPUT
### Fixes applied:
- Cải thiện bảo mật luồng đăng nhập. Dừng lưu trữ dữ liệu cá nhân dư thừa (email, tên) trên trình duyệt web của người dùng để đảm bảo an toàn thông tin.

## OUTPUT
Release Note (Draft) - user-facing 
```

# 2. AI Output (Release Note Draft)
## Release version:

## Bug Fixes: 
Bản cập nhật sắp tới sẽ khắc phục hoàn toàn vấn đề lưu vết thông tin người dùng ngoài ý muốn trên trình duyệt web. Quá trình đăng nhập của bạn sẽ được bảo vệ an toàn và riêng tư hơn sau khi bản vá này được phát hành.

## Cải tiến: 
Hệ thống sẽ tự động ngừng lưu trữ các thông tin cá nhân không cần thiết (như email và họ tên) trên thiết bị của bạn. Thay đổi này sẽ giúp nâng cấp tối đa lớp rào chắn bảo mật để bảo vệ dữ liệu của bạn an toàn tuyệt đối.

## Known Issues: 
Hiện tại chúng tôi chưa ghi nhận bất kỳ vấn đề phát sinh nào cho phiên bản chuẩn bị ra mắt này.