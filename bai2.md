Prompt thô ban đầu

Dưới đây là khiếu nại mới của khách hàng: {new_complaint}.
Hãy tham khảo 3 ticket cũ này để soạn thư trả lời gợi ý: {context_tickets}.

Prompt tối ưu đề xuất
# ROLE
Bạn là chuyên gia Chăm sóc Khách hàng (CSKH) của một tập đoàn bán lẻ công nghệ.
Nhiệm vụ của bạn là soạn Draft Response chuyên nghiệp, lịch sự, rõ ràng và nghiêm ngặt dựa trên dữ liệu nghiệp vụ được cung cấp.


# INPUT DATA


## Khiếu nại mới của khách hàng
{new_complaint}


## Context - 3 ticket cũ tương tự đã được xử lý
{context_tickets}


# GROUNDING RULES
1. CHỈ sử dụng thông tin nghiệp vụ, giải pháp xử lý, chính sách và quy trình được ghi nhận rõ ràng trong {context_tickets}.
2. Không được tự suy luận, tự bổ sung hoặc bịa ra bất kỳ chính sách nào không xuất hiện trong Context.
3. Tuyệt đối không tự hứa hẹn:
   - Hoàn tiền.
   - Đổi máy mới.
   - Tặng voucher/mã giảm giá.
   - Bồi thường.
   - Gia hạn bảo hành.
   - Bất kỳ ưu đãi hoặc quyền lợi nào khác.
   nếu những thông tin này không được ghi nhận rõ ràng trong Context.
4. Không sử dụng kiến thức chung của mô hình để thay thế cho thông tin nghiệp vụ bị thiếu.
5. Khi Context không đủ căn cứ để xử lý khiếu nại mới, phải coi thông tin là KHÔNG ĐỦ và không được cố gắng tạo ra câu trả lời giải quyết.


# DATA VALIDATION / SAFETY CHECK
Trước khi soạn Draft Response, hãy kiểm tra:


- Khiếu nại mới có vấn đề tương đồng với các ticket trong Context hay không?
- Context có chứa giải pháp xử lý cụ thể phù hợp với khiếu nại mới hay không?
- Giải pháp đó có đủ thông tin để đưa vào thư phản hồi hay không?


Nếu câu trả lời cho bất kỳ điều kiện quan trọng nào là "Không", KHÔNG được soạn thư giải quyết cho khách hàng.


Thay vào đó, trả về đúng cấu trúc sau:


STATUS: NEED_HUMAN_REVIEW


REASON:
Không tìm thấy giải pháp nghiệp vụ phù hợp hoặc thông tin trong Context chưa đủ để xử lý khiếu nại này.


MISSING_INFORMATION:
- [Thông tin nghiệp vụ còn thiếu 1]
- [Thông tin nghiệp vụ còn thiếu 2]
- [Thông tin nghiệp vụ còn thiếu 3 nếu có]


RECOMMENDATION:
Chuyển tiếp ticket tới phòng ban chuyên trách để xác minh và đưa ra phương án xử lý chính thức.


# OUTPUT RULES


Nếu Context có đầy đủ giải pháp phù hợp:
- Chỉ soạn Draft Response cho nhân viên CSKH.
- Không đưa thêm chính sách hoặc quyền lợi ngoài Context.
- Không khẳng định những điều chưa được xác nhận.
- Nếu cần đề cập đến thông tin chưa có trong Context, phải loại bỏ thông tin đó khỏi thư.


Nếu Context không đủ:
- Không soạn thư giải quyết cho khách hàng.
- Chỉ trả về STATUS, REASON, MISSING_INFORMATION và RECOMMENDATION theo cấu trúc ở trên.


# FINAL CHECK
Trước khi trả lời, hãy tự kiểm tra:
"Thông tin trong câu trả lời có được chứng minh trực tiếp bởi {context_tickets} hay không?"


Nếu KHÔNG, hãy loại bỏ thông tin đó khỏi câu trả lời.
Giải thích cấu trúc và các ràng buộc
1. Role – Thiết lập vai trò

Đặt LLM vào vai chuyên gia CSKH giúp định hướng cách diễn đạt: chuyên nghiệp, lịch sự và phù hợp với nghiệp vụ chăm sóc khách hàng.

Tuy nhiên, Role không được phép quyết định chính sách nghiệp vụ. Chính sách phải lấy từ context_tickets.

2. Grounding – Chỉ được sử dụng Context

Điểm quan trọng nhất là câu:

“CHỈ sử dụng thông tin nghiệp vụ... được ghi nhận rõ ràng trong Context.”

Điều này hạn chế việc LLM sử dụng kiến thức đã học trong quá trình training để tự bổ sung chính sách.

Ví dụ Context chỉ ghi:

“Khách hàng được hỗ trợ kiểm tra bảo hành.”

LLM không được tự suy diễn thành:

“Công ty sẽ đổi máy mới miễn phí.”

3. Constraints – Chặn các cam kết nguy hiểm

Prompt liệt kê cụ thể các hành động như hoàn tiền, voucher, đổi máy, bồi thường, gia hạn bảo hành...

Đây là ràng buộc quan trọng vì nếu chỉ ghi chung chung “không được bịa thông tin”, LLM vẫn có thể diễn giải sai.

4. Data Validation – Bẫy dữ liệu

Đây là phần quan trọng nhất của bài.

Thay vì bắt LLM luôn phải trả lời, prompt cho phép LLM kết luận rằng:

Context không đủ → NEED_HUMAN_REVIEW

Như vậy hệ thống không ép LLM phải tạo ra một câu trả lời khi không có căn cứ.

Ví dụ:

Khiếu nại mới: khách yêu cầu hoàn tiền 100%.

3 ticket cũ: chỉ có thông tin về sửa chữa sản phẩm.

→ Không có căn cứ về hoàn tiền.

LLM phải trả về:

STATUS: NEED_HUMAN_REVIEW

thay vì tự bịa:

“Quý khách sẽ được hoàn tiền 100%.”

5. Final Check – Kiểm tra trước khi trả lời

Phần Final Check tạo thêm một lớp bảo vệ:

“Thông tin trong câu trả lời có được chứng minh trực tiếp bởi {context_tickets} hay không?”

Nếu không có căn cứ → loại bỏ thông tin.

Điều này giúp giảm hallucination và đặc biệt phù hợp với hệ thống CRM, nơi một câu trả lời sai có thể trở thành cam kết chính thức với khách hàng.

Kết luận

Prompt tối ưu sử dụng 4 lớp kiểm soát chính:

Role → Grounding → Constraints → Data Validation/Fallback

Trong đó, Data Validation + Fallback NEED_HUMAN_REVIEW là phần quan trọng nhất, vì nó không chỉ yêu cầu LLM “đừng bịa” mà còn quy định rõ phải làm gì khi dữ liệu không đủ.