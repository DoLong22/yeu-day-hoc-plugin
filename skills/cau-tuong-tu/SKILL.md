---
name: cau-tuong-tu
description: Soạn câu tương tự (biến thể) từ một câu đã có trong ngân hàng Yêu Dạy Học — giữ dạng, nhãn, mức độ, đổi số liệu hoặc ngữ cảnh. Dùng khi cô muốn thêm câu giống một câu cũ.
---

# Câu tương tự từ câu có sẵn

Trình bày theo mục **TRÌNH BÀY — ÍT CHỮ** trong chỉ dẫn của máy chủ Yêu Dạy Học (một nguồn luật): thẻ có màu thay bảng — **thẻ đã hiện thì KHÔNG chép lại** bảng, ma trận, đề hay lời giải (làm theo dòng đầu kết quả tool), ≤ 5 dòng chữ mỗi lượt, kết thúc bằng một câu hỏi, không kể lại việc đã làm.

## Các bước
1. Tìm câu gốc: cô đưa id → `get_question`; cô mô tả → `search_questions` rồi hỏi cô chọn đúng câu (danh sách 3–4 kết quả, mỗi câu một dòng rút gọn).
2. Gọi `get_lesson_context` cho câu gốc: theo mã yêu cầu cần đạt của câu (`codes`) nếu câu có, không thì theo nhãn (`tagId`); kèm `subjectKey`, `grade`. Có nội dung → biến thể phải nằm trong khung đó. Không có → chỉ đổi số liệu/ngữ cảnh, **không** thêm kiến thức mới.
3. Nhiều biến thể (≥ 4 câu) → trình kế hoạch bằng `show_question_plan` (không tự vẽ bảng). Giữ nguyên: **dạng câu, nhãn, mức độ**, cấu trúc lời giải. Đổi: số liệu, ngữ cảnh, tên riêng, thứ tự phương án. Phương án nhiễu vẫn là lỗi hay gặp. Câu gốc có hình → vẽ lại hình mới theo **số liệu mới** (luật HÌNH THAY LỜI), gửi kèm `image`.
4. Mỗi biến thể có lời giải đầy đủ + `grounding` (vd. "Biến thể của câu … — cùng ý: đổi đơn vị vận tốc") + `requirementCodes` (cùng mã với câu gốc / mã từ `get_lesson_context`).
5. Tự kiểm từng câu như skill soan-cau-hoi Bước 6 (giải độc lập, tính bằng code, đơn vị, đúng một đáp án, số liệu hợp lý).
6. Gọi `preview_questions` (`source: "ai_generated"`) → cô duyệt trên **thẻ** (chỉ dán `detail` khi kết quả báo không hiện thẻ); thêm **một dòng** "đổi gì so với câu gốc". **Chờ cô đồng ý.** Cô sửa → `preview_questions` lại.
7. Cô bấm nút **Lưu** trên thẻ (kết quả/ngữ cảnh báo "Cô đã bấm Lưu trên thẻ") → đã lưu: **không** gọi `create_questions` nữa, chỉ xác nhận một dòng + link. Cô trả lời đồng ý trong chat → lưu bằng `create_questions(previewId)` (hình gửi kèm được tạo luôn), báo một dòng + reviewUrl. Chỉ **cô** duyệt — em không bao giờ tự duyệt (không có tool duyệt). Cô bấm **"Duyệt cả N câu"** trên thẻ (sau khi lưu) hoặc duyệt trong app. Kết quả/ngữ cảnh báo "Cô đã duyệt N câu trên thẻ" → câu đã vào ngân hàng, dùng được ngay; chưa có tin đó thì đừng nói câu đã dùng được.
