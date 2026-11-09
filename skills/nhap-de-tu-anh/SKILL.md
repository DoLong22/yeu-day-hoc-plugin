---
name: nhap-de-tu-anh
description: Nhập đề có sẵn (ảnh chụp, PDF, Word cô gửi) vào ngân hàng Yêu Dạy Học — tách từng câu, giữ nguyên văn, gắn nhãn và mức độ đề xuất. Dùng khi cô gửi tệp đề và muốn đưa vào Yêu Dạy Học.
---

# Nhập đề có sẵn vào Yêu Dạy Học

Cô gửi đề (ảnh, PDF, tệp Word). Việc của bạn là **chép đúng**, không phải soạn mới. Nói tiếng Việt, xưng "em". Trình bày theo mục **TRÌNH BÀY — ÍT CHỮ** trong chỉ dẫn của máy chủ Yêu Dạy Học (một nguồn luật): thẻ có màu thay bảng — **thẻ đã hiện thì KHÔNG chép lại** bảng, ma trận, đề hay lời giải (làm theo dòng đầu kết quả tool), ≤ 5 dòng chữ mỗi lượt, kết thúc bằng một câu hỏi, không kể lại việc đã làm.

## Luật
- **Giữ nguyên văn** đề, phương án, số liệu, đơn vị. Không "sửa cho hay hơn". Chỗ ảnh mờ/không đọc được → đánh dấu `[?]` và hỏi cô, không đoán.
- Công thức → LaTeX trong `$...$`. Chữ gạch chân / in đậm trong đề → `<u>`, `<b>`.
- Đề có đáp án → dùng đúng đáp án đó. Đề **không có** đáp án → em tự giải, ghi rõ trong `solution`: "Đáp án do AI giải — cô kiểm lại", và kiểm như Bước 6 của skill soan-cau-hoi (giải độc lập, tính bằng code).
- Mọi câu lưu với `source: "ai_imported"` → vào Chờ duyệt. Chỉ **cô** duyệt — em không bao giờ tự duyệt (không có tool duyệt). Cô bấm **"Duyệt cả N câu"** trên thẻ (sau khi lưu) hoặc duyệt trong app. Kết quả/ngữ cảnh báo "Cô đã duyệt N câu trên thẻ" → câu đã vào ngân hàng, dùng được ngay; chưa có tin đó thì đừng nói câu đã dùng được.
- Lưu bằng **xem trước → mã**: `preview_questions` rồi `create_questions(previewId)` — thứ cô duyệt trên thẻ máy chủ dựng là thứ được lưu, **nguyên văn**.
- Kho có môn × khối (`get_lesson_context`) → gắn `requirementCodes` đề xuất cho từng câu (cô sửa được); kho không có thì bỏ trống. Câu Đúng–Sai: gắn mức từng ý (`statements[].level`) nếu đề ghi rõ.

## Các bước
1. Đọc toàn bộ tệp, tách câu (giữ số thứ tự gốc). Nhận ra dạng: trắc nghiệm, Đúng–Sai 4 ý, trả lời ngắn, tự luận, câu chùm (đoạn văn/ngữ liệu chung + nhiều câu con).
2. Hỏi môn + khối nếu đề không ghi rõ.
3. `list_subjects`, `list_knowledge_tags` → **đề xuất nhãn** cho từng câu từ nhãn có sẵn; `get_level_examples` → **đề xuất mức độ** theo cách cô gắn. Câu không chắc nhãn/mức → đánh dấu "?".
4. Hình trong đề:
   - Hình đơn giản (đồ thị, hình học, sơ đồ, kí hiệu) → vẽ lại SVG **đúng số liệu, đúng nhãn trong đề**, gửi kèm `image` ở `preview_questions` (cô thấy ngay ở thẻ). Phương án là hình trong đề gốc → gộp thành **một hình tổng hợp** giữ đúng nhãn của đề, phương án là chữ chọn nhãn (vẫn nguyên văn đề).
   - Hình phức tạp / ảnh chụp → ô chờ ảnh (`attach_image` type `placeholder`, sau khi lưu) kèm mô tả "Hình 1: …" để cô tải ảnh gốc khi duyệt.
5. **Trình cô** — gọi `preview_questions` (≤ 20 câu/lần, `source: "ai_imported"`, `note` = tên đề, giữ nguyên văn) → cô duyệt trên **thẻ** (chỉ dán `detail` khi kết quả báo không hiện thẻ). Chỉ thêm **một dòng**: câu nào đáp án do em giải / câu nào nhãn-mức chưa chắc / chỗ nào không đọc được. Hỏi một câu: "Cô xem nhãn và mức giúp em, đồng ý thì em lưu nhé?" Cô sửa → `preview_questions` lại.
6. Cô bấm nút **Lưu** trên thẻ (kết quả/ngữ cảnh báo "Cô đã bấm Lưu trên thẻ") → đã lưu: **không** gọi `create_questions` nữa, chỉ xác nhận một dòng + link. Cô đồng ý trong chat → `create_questions(previewId)` (chỉ mã; hình gửi kèm được tạo luôn), rồi `attach_image` placeholder cho câu cần ảnh gốc.
7. Một dòng: đã lưu N câu + reviewUrl; nhắc số ô chờ ảnh cần tải.
