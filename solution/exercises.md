# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 4 tiếng
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
<!-- > *Câu trả lời của bạn* -->
> Khi temperature tăng, các response trở nên đa dạng hơn và khó dự đoán hơn. Ở temperature thấp hơn (0.0–1.0), các phản hồi tương đối nhất quán và mạch lạc, trong khi ở 1,5, phản hồi trở nên ngẫu nhiên hơn và ít liên quan hơn. Điều này cho thấy rằng temperature cao hơn nhìn chung làm tăng tính ngẫu nhiên của response được tạo ra.


### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
<!-- > *Câu trả lời của bạn* -->

> Tôi sẽ đặt temperature khoảng 0,3–0,5 cho chatbot hỗ trợ khách hàng. Temperature thấp hơn giúp các phản hồi nhất quán, dễ dự đoán và đáng tin cậy hơn, điều này rất quan trọng khi cung cấp thông tin và hỗ trợ cho khách hàng.


### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
<!-- > *Câu trả lời của bạn* -->

> - 10,000 users × 3 calls = 30,000 calls/day
> - 30,000 × 350 = 10,500,000 output tokens/day
> - GPT-4o: 10.5M / 1,000 × $0.010 = $105/day
> - GPT-4o-mini: 10.5M / 1,000 × $0.0006 = $6.30/day
> - GPT-4o đắt hơn khoảng 16.7 lần.
> - GPT-4o xứng đáng với các tác vụ phức tạp đòi hỏi khả năng suy luận chất lượng cao hoặc phản hồi chính xác, trong khi GPT-4o-mini phù hợp hơn cho các tác vụ đơn giản, khối lượng lớn như trả lời FAQ hoặc hỗ trợ khách hàng cơ bản.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
<!-- > *Câu trả lời của bạn* -->
> Với vai trò giáo viên tiểu học, câu trả lời sử dụng ngôn ngữ đơn giản, ví dụ quen thuộc và cách giải thích dễ hiểu, phù hợp với trẻ em. Câu trả lời cũng hạn chế sử dụng các thuật ngữ kỹ thuật phức tạp. Trong khi đó, với vai trò chuyên gia tài chính, câu trả lời sử dụng nhiều thuật ngữ chuyên môn hơn như “Distributed Ledger Technology (DLT)”, “cryptography”, “decentralization” và “trustless system”. Cách giải thích mang tính học thuật, chuyên sâu và tập trung nhiều hơn vào các khía cạnh kỹ thuật và tài chính của blockchain. Điều này cho thấy system prompt có ảnh hưởng đáng kể đến từ vựng, giọng điệu, mức độ chi tiết và phong cách giải thích của mô hình.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
<!-- > *Câu trả lời của bạn* -->

> Với đoạn văn tiếng Việt gồm 109 từ. Hai phương pháp chênh nhau khoảng 11.9%. Cụ thể, phương pháp số từ / 0.75 ước lượng 145.33 token, trong khi count_tokens() cho kết quả 128 token. Tiếng Việt có thể tốn nhiều token hơn tiếng Anh khi sử dụng một số tokenizer vì tokenizer thường được tối ưu và huấn luyện nhiều hơn trên dữ liệu tiếng Anh. Các từ tiếng Việt có dấu và cách tách từ bằng khoảng trắng cũng có thể khiến một từ được chia thành nhiều token hơn. Vì vậy, cùng một nội dung, số token của tiếng Việt có thể cao hơn tiếng Anh. Tuy nhiên, điều này phụ thuộc vào tokenizer cụ thể và không phải lúc nào tiếng Việt cũng sử dụng nhiều token hơn tiếng Anh.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
<!-- > *Câu trả lời của bạn* -->
> Streaming quan trọng nhất khi mô hình cần tạo ra phản hồi dài hoặc mất nhiều thời gian xử lý, chẳng hạn như chatbot, trợ lý AI hoặc tạo nội dung dài. Người dùng có thể nhìn thấy từng phần của câu trả lời ngay khi chúng được tạo ra thay vì phải chờ toàn bộ response, từ đó cải thiện cảm giác về tốc độ phản hồi. Ngược lại, non-streaming phù hợp hơn khi ứng dụng cần nhận toàn bộ kết quả trước khi xử lý tiếp, chẳng hạn như các tác vụ API, xử lý dữ liệu hoặc khi response ngắn và thời gian chờ không đáng kể.


### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
<!-- > *Câu trả lời của bạn* -->
> Exponential backoff giúp giảm tải cho API khi xảy ra quá tải hoặc lỗi tạm thời. Thay vì tất cả client đều retry sau cùng một khoảng thời gian cố định, thời gian chờ sẽ tăng dần sau mỗi lần retry, ví dụ 0.1s, 0.2s, 0.4s, 0.8s. Nếu hàng nghìn client cùng retry với delay cố định 1 giây, chúng có thể gửi request lại gần như cùng lúc, tạo ra một “thundering herd” và tiếp tục làm API quá tải. Exponential backoff giúp phân tán các lần retry theo thời gian, giảm áp lực lên hệ thống và tăng khả năng API phục hồi.


---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
<!-- > *Câu trả lời của bạn* -->
> Tôi chọn persona **trợ lý hỗ trợ học lập trình**.
> System prompt:
> “Bạn là một trợ lý lập trình thân thiện. Hãy giúp người dùng học và hiểu code bằng cách giải thích đơn giản, rõ ràng và đưa ra ví dụ ngắn khi cần. Trả lời bằng tiếng Việt và tránh sử dụng thuật ngữ quá phức tạp nếu không cần thiết.” 
> Tôi chọn từ **“đơn giản, rõ ràng”** để trợ lý tập trung vào việc giúp người mới hiểu code thay vì đưa ra những giải thích quá phức tạp. Tôi cũng chỉ định **“trả lời bằng tiếng Việt”** để câu trả lời dễ hiểu và phù hợp với người dùng.


### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
<!-- > *Câu trả lời của bạn* -->
> Hạn chế lớn nhất của trợ lý hiện tại là history chỉ lưu một số lượt hội thoại gần nhất. Khi cuộc trò chuyện trở nên dài, trợ lý có thể mất thông tin quan trọng từ các lượt trao đổi trước đó. Một cải thiện cụ thể là sử dụng summary-based memory. Sau một số lượt hội thoại, hệ thống có thể tóm tắt các thông tin quan trọng từ history và lưu bản tóm tắt này. Khi gửi request mới, summary sẽ được đưa vào system prompt hoặc context cùng với các lượt hội thoại gần nhất. Cách này giúp trợ lý duy trì được thông tin quan trọng trong các cuộc hội thoại dài mà không cần gửi toàn bộ history cho mỗi request.

---

## Danh Sách Kiểm Tra Nộp Bài

- [x] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [x] Cả 4 checkpoint pytest đều pass
- [x] Tất cả 9 câu trong file này đã được trả lời
- [x] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
