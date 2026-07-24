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
Em nhận thấy khi temperature tăng, cách diễn đạt của mô hình có xu hướng đa dạng, chi tiết và sáng tạo hơn. Tuy nhiên, nội dung chính giữa bốn phản hồi vẫn khá giống nhau vì mô hình đều chọn sự thật về hang Sơn Đoòng; ở mức temperature thấp, câu trả lời ổn định hơn, còn mức cao dễ dài dòng và kém nhất quán hơn, có thể xuất hiện các chi tiết không liên quan.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
Em sẽ đặt temperature ở mức 0.3-0.5 để đảm bảo câu trả lời ổn định, chính xác và ít rủi ro về thông tin sai lệch, đồng thời vẫn giữ được sự linh hoạt và thân thiện trong giao tiếp với khách hàng.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
Em ước tính GPT-4o đắt hơn GPT-4o-mini khoảng 5 lần cho workload này. Trường hợp GPT-4o xứng đáng với chi phí là khi cần xử lý các tác vụ phức tạp, yêu cầu độ chính xác cao và khả năng hiểu ngữ cảnh tốt, ví dụ như phân tích dữ liệu tài chính hoặc y tế. Ngược lại, nên dùng mini khi các tác vụ đơn giản, không yêu cầu độ chính xác cao, ví dụ như trả lời câu hỏi thông tin cơ bản hoặc hỗ trợ khách hàng với các vấn đề phổ biến.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
Hai phản hồi khác nhau rõ rệt về độ dài, từ vựng và cách giải thích. System prompt ảnh hưởng đến hành vi model bằng cách định hướng cách trả lời, từ ngữ và mức độ chuyên sâu.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
Hai con số chênh nhau khoảng 20-30%. Tiếng Việt thường tốn nhiều token hơn tiếng Anh cùng độ dài vì tiếng Việt có nhiều từ ghép và dấu câu, dẫn đến việc phân tách từ và token hóa phức tạp hơn.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
Streaming rất quan trọng khi cần cung cấp phản hồi nhanh chóng cho người dùng, đặc biệt là trong các ứng dụng tương tác thời gian thực. Trong khi đó, non-streaming phù hợp hơn khi cần xử lý và hiển thị toàn bộ phản hồi trước khi cho người dùng xem.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
Exponential backoff giúp giảm tải cho API bằng cách tăng dần thời gian chờ giữa các lần retry, tránh tình trạng quá tải khi nhiều client cùng retry đồng thời. Nếu hàng nghìn client cùng retry với delay cố định giống nhau, sẽ dẫn đến hiện tượng "thundering herd", gây ra quá tải và làm giảm hiệu suất của hệ thống.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
Em chọn persona là "trợ lý học tập thông minh, thân thiện và hỗ trợ sinh viên trong việc học tập và nghiên cứu." System prompt: "Bạn là một trợ lý học tập thông minh, thân thiện, luôn cung cấp thông tin chính xác và hữu ích cho sinh viên. Hãy trả lời ngắn gọn, rõ ràng và bằng tiếng Việt." Lựa chọn từ ngữ "trả lời ngắn gọn" nhằm đảm bảo rằng phản hồi không quá dài dòng, giúp sinh viên dễ dàng tiếp nhận thông tin. Việc chỉ định ngôn ngữ là tiếng Việt để phù hợp với đối tượng người dùng chính là sinh viên Việt Nam.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
Hạn chế lớn nhất của trợ lý là không có bộ nhớ dài hạn, dẫn đến việc không thể ghi nhớ các cuộc trò chuyện trước đó và cung cấp phản hồi dựa trên ngữ cảnh lâu dài. Một cải thiện cụ thể là triển khai một cơ chế lưu trữ lịch sử trò chuyện trong cơ sở dữ liệu, cho phép trợ lý truy xuất thông tin từ các cuộc trò chuyện trước đó khi cần thiết. Cách triển khai có thể bao gồm việc lưu trữ các câu hỏi và câu trả lời vào cơ sở dữ liệu sau mỗi lượt trò chuyện, và khi người dùng bắt đầu một cuộc trò chuyện mới, trợ lý có thể truy vấn cơ sở dữ liệu để lấy thông tin liên quan từ các cuộc trò chuyện trước đó.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/` và zip theo hướng dẫn README
