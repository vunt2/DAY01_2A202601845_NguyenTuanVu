# K3 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 9h00–13h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng placeholder bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi temperature = 0.0, phản hồi mang tính logic, chính xác và cố định giữa các lần gọi. Khi tăng lên 0.5 và 1.0, từ ngữ trở nên phong phú, tự nhiên và đa dạng hơn. Tuy nhiên khi lên mức 1.5, phản hồi bắt đầu bị lan man, lặp từ hoặc bịa đặt thông tin sai sự thật (hallucination).

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi sẽ đặt temperature ở mức thấp từ 0.0 đến 0.2. Lý do là chatbot hỗ trợ khách hàng cần cung cấp thông tin chính xác, nhất quán về chính sách, giá cả và thông tin sản phẩm, tránh tuyệt đối việc tự sáng tạo ra thông tin không có thật gây hiểu lầm cho khách hàng.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> - Chi phí GPT-4o ($0.010/1K token) đắt hơn GPT-4o-mini ($0.0006/1K token) khoảng 16.7 lần cho workload này (khoảng $105/ngày so với $6.3/ngày).
> - Trường hợp dùng GPT-4o: Cần xử lý các tác vụ phức tạp đòi hỏi suy luận sâu như viết mã nguồn nâng cao, phân tích hợp đồng pháp lý hoặc giải toán phức tạp.
> - Trường hợp nên dùng GPT-4o-mini: Dùng cho chatbot trả lời câu hỏi thường gặp (FAQ), tóm tắt văn bản ngắn, hoặc phân loại dữ liệu đơn giản.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Hai phản hồi khác biệt hoàn toàn về góc nhìn và vốn từ. Với persona giáo viên tiểu học, câu trả lời ngắn gọn, sử dụng từ ngữ đơn giản và ẩn dụ gần gũi như "cuốn sổ tay diệu kỳ được truyền tay nhau". Trong khi đó, persona chuyên gia tài chính đưa ra phản hồi dài hơn, tập trung vào tính phân tán (decentralized), sổ cái mã hóa (cryptographic ledger) và cơ chế đồng thuận. Điều này cho thấy System prompt đóng vai trò như bộ lọc định hướng văn phong, độ phức tạp và trọng tâm thông tin cho toàn bộ phản hồi của mô hình.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Số token đếm thật bằng tiktoken cao hơn từ 50% đến 80% so với ước lượng (số từ / 0.75). Nguyên nhân là bộ mã hóa BPE (Byte-Pair Encoding) của OpenAI được huấn luyện tối ưu chủ yếu cho tiếng Anh. Tiếng Việt có các từ ghép và dấu thanh (sắc, huyền, hỏi, ngã, nặng) nên thường bị thuật toán phân tách thành nhiều sub-word token thay vì 1 token trọn vẹn như tiếng Anh.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất trong các ứng dụng chatbot hội thoại và giao diện tương tác trực tiếp với người dùng, nơi cần giảm thiểu thời gian chờ chữ xuất hiện đầu tiên (Time to First Token - TTFT) để mang lại cảm giác mượt mà và trực quan. Ngược lại, non-streaming lại phù hợp hơn khi ứng dụng cần xử lý ngầm (batch jobs), cần cấu trúc dữ liệu JSON hoàn chỉnh để trích xuất dữ liệu (Function Calling / API integration), hoặc thực hiện các bài đánh giá tự động mà không cần hiển thị trực tiếp giao diện cho người dùng.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giúp giãn khoảng cách thời gian giữa các lần thử lại theo cấp số nhân (ví dụ: 0.1s, 0.2s, 0.4s...), từ đó tạo ra khoảng lặng thời gian để hệ thống/server bị nghẽn có cơ hội phục hồi. Nếu hàng nghìn client cùng retry với một khoảng thời gian cố định giống nhau (ví dụ 1 giây), chúng sẽ đồng loạt gửi lại request tại cùng một thời điểm, gây ra hiện tượng "Thảm họa dồn dập" (Thundering Herd Problem), khiến server bị quá tải liên tục và không thể khôi phục trạng thái bình thường.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Tôi chọn persona là một "Trợ giảng AI thân thiện chuyên ngành Lập trình". System prompt: "Bạn là trợ giảng lập trình AI thân thiện. Hãy trả lời ngắn gọn, súc tích bằng tiếng Việt, kèm theo ví dụ minh họa bằng code ngắn nếu cần."
> - Lựa chọn "trả lời ngắn gọn": Giúp giảm độ trễ và tiết kiệm token (chi phí API).
> - Lựa chọn "bằng tiếng Việt": Đảm bảo trải nghiệm người dùng nhất quán, tránh việc LLM tự động trả lời bằng tiếng Anh.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
 thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất hiện tại là lịch sử hội thoại chỉ duy trì cố định 3 lượt cuối (`history[-6:]`), khiến bot quên hoàn toàn thông tin người dùng đã cung cấp ở các lượt trước đó nếu cuộc trò chuyện kéo dài.
> - Đề xuất cải thiện: Triển khai cơ chế "Conversation Summary Memory". 
> - Cách triển khai: Khi độ dài history vượt quá 6 message, thay vì cắt bỏ thẳng tay, gọi một lời gọi API ngầm để tóm tắt các đoạn chat cũ thành một đoạn văn tóm tắt ngắn, sau đó nối đoạn tóm tắt này vào System Prompt để bảo toàn bộ nhớ dài hạn cho mô hình.

---

## Danh Sách Kiểm Tra Nộp Bài

- [x] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [x] Cả 4 checkpoint pytest đều pass
- [x] Tất cả 9 câu trong file này đã được trả lời
- [x] Đã copy bài làm vào folder `solution/` và zip theo hướng dẫn README
