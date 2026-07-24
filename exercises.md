# K3 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 9h00–13h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi tempurature thấp sẽ ổn định
> Nếu muốn đa dạng sẽ nâng tempurature lên nhưng cao quá sẽ diễn đạt dài dòng

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> 0.2 và 0.3 vì cần chính xác và nhất quán

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> khoảng 16 - 17 lần theo bảng giá phù hợp phân tích tài liệu chuyên sâu
> GPT-4o-mini phù hợp theo kiểu chăm sóc khách hàng

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> system prompt định hướng vai trò, cách diễn đạt, mức độ chi tiết và đối tượng mà mô hình hướng tới khi trả lời.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Với một đoạn văn khoảng 100 từ tiếng Việt, count_tokens (tiktoken) thường cho kết quả khoảng 140–150 token, trong khi cách ước lượng số từ / 0.75 cho khoảng 133 token, chênh lệch khoảng 5–10%. Sự khác biệt xuất phát từ việc tokenizer chia văn bản thành các đơn vị nhỏ hơn từ hoàn chỉnh, đặc biệt với tiếng Việt có dấu, từ ghép và ký tự Unicode, nên thường tiêu tốn nhiều token hơn tiếng Anh có cùng độ dài.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> khi mô hình tạo ra câu trả lời dài hoặc cần thời gian xử lý, vì người dùng có thể nhìn thấy nội dung xuất hiện ngay thay vì phải chờ toàn bộ phản hồi hoàn thành.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giúp giảm số lượng yêu cầu gửi lại trong thời gian ngắn khi API đang quá tải, từ đó tăng khả năng yêu cầu thành công ở các lần thử tiếp theo. So với việc luôn chờ một khoảng thời gian cố định, backoff theo cấp số nhân phân tán thời điểm retry của các client và giảm áp lực lên máy chủ. Nếu hàng nghìn client cùng retry sau đúng 1 giây, chúng có thể đồng loạt gửi yêu cầu trở lại, tạo ra tình trạng quá tải lặp đi lặp lại (thường gọi là "retry storm") và khiến hệ thống khó phục hồi.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> để người dùng dễ đọc và nhanh nắm được ý chính. Việc chỉ định trả lời bằng tiếng việt giúp đảm bảo ngôn ngữ thống nhất

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất của trợ lý hiện tại là chỉ lưu được 3 lượt hội thoại gần nhất, nên dễ quên các thông tin đã trao đổi từ trước nếu cuộc trò chuyện kéo dài. Một cải thiện phù hợp là bổ sung bộ nhớ dài hạn bằng cách lưu lịch sử hội thoại vào cơ sở dữ liệu hoặc tệp, sau đó truy xuất những nội dung liên quan khi người dùng đặt câu hỏi mới. Điều này sẽ giúp trợ lý duy trì ngữ cảnh tốt hơn và đưa ra câu trả lời nhất quán trong các cuộc hội thoại dài.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/` và zip theo hướng dẫn README
