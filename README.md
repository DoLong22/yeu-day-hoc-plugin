# Yêu Dạy Học — plugin cho Claude

Plugin giúp thầy cô dạy học ở Việt Nam dùng Claude soạn câu hỏi và đề kiểm tra thẳng vào ngân hàng câu hỏi của mình trên
[Yêu Dạy Học](https://yeudayhoc.vn). Claude tra yêu cầu cần đạt của chương trình GDPT 2018 và mục lục bộ sách, hỏi thầy cô trước khi soạn,
tự kiểm từng câu, rồi lưu vào mục **Chờ duyệt**. Không câu nào được dùng cho tới khi thầy cô duyệt.

## Lệnh

| Lệnh | Việc |
|---|---|
| `/soan-cau` | Soạn câu hỏi mới theo một bài hoặc chủ đề (trắc nghiệm, Đúng – Sai, trả lời ngắn, tự luận) |
| `/soan-de` | Ghép đề kiểm tra theo ma trận mức độ từ câu đã duyệt (lưu dạng đề nháp) |
| `/nhap-de` | Nhập đề có sẵn từ ảnh, PDF, Word — giữ nguyên văn |
| `/cau-tuong-tu` | Soạn câu tương tự một câu đã có trong ngân hàng |

Không gõ lệnh cũng được: nói tự nhiên, ví dụ “soạn 10 câu trắc nghiệm bài Chuyển động đều, KHTN lớp 7”.

## Cần có

- Tài khoản Yêu Dạy Học (đăng ký tại https://app.yeudayhoc.vn).
- Claude trên web hoặc ứng dụng máy tính. Dùng được với mọi gói, kể cả gói Free.

## Cài đặt

1. claude.ai → Customize → Plugins → Add → Add marketplace → Add from a repository → dán địa chỉ kho này → cài plugin.
2. Mở plugin → tab Connectors → Connect → đăng nhập Yêu Dạy Học → Cho phép.
3. Hướng dẫn có hình: https://app.yeudayhoc.vn/ai-connections/guide?ai=claude

Claude Code (dòng lệnh, bản 2.1.275 trở lên):

```
/plugin install yeu-day-hoc --marketplace DoLong22/yeu-day-hoc-plugin
```

## Plugin kết nối và gửi gì

- Plugin chỉ gồm văn bản hướng dẫn (skills, lệnh) và **một** kết nối MCP từ xa tới máy chủ Yêu Dạy Học: `https://app.yeudayhoc.vn/api/mcp`.
- Đăng nhập bằng OAuth 2.0 — thầy cô tự đăng nhập Yêu Dạy Học và bấm Cho phép; plugin không chứa và không đọc mật khẩu hay khoá nào.
- Dữ liệu gửi tới máy chủ: nội dung câu hỏi, đề, nhãn kiến thức, phạm vi bài học thầy cô chọn, và ảnh hình vẽ của câu (nếu có).
- Máy chủ trả về: khung kiến thức của bài, câu mẫu của thầy cô, câu đã duyệt để ghép đề.
- Kết nối **không** có quyền xem học sinh, lớp hay điểm. Yêu Dạy Học không lưu nội dung cuộc trò chuyện.
- Plugin không chạy chương trình nào trên máy thầy cô và không gửi dữ liệu tới nơi nào khác.

## Quyền riêng tư, hỗ trợ

- Chính sách quyền riêng tư: https://yeudayhoc.vn/chinh-sach-quyen-rieng-tu
- Điều khoản sử dụng: https://yeudayhoc.vn/dieu-khoan
- Hỗ trợ: https://yeudayhoc.vn/ho-tro · doduclong2208@gmail.com

## Giấy phép

MIT — xem tệp LICENSE. Giấy phép áp cho các tệp trong kho này; dịch vụ Yêu Dạy Học có điều khoản riêng.

Phiên bản 1.4.0. Kho này được sinh tự động từ mã nguồn Yêu Dạy Học (`pnpm plugin:repo`) — đừng sửa tay.
