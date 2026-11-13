# BÀI 12: QUY TRÌNH ONBOARDING NHÂN VIÊN TƯ VẤN BÁN HÀNG TECHSHOP ĐA TẦNG

## PHẦN 1. XÁC ĐỊNH YÊU CẦU BÀI TẬP

### 1.1. Bài tập yêu cầu làm gì?

TechShop tuyển nhân viên bán hàng mới nhưng nhiều bạn chưa có kinh nghiệm xử lý khi khách hàng chê giá đắt hoặc so sánh giá với Shopee, Lazada.

Bài tập yêu cầu xây dựng một chatbot AI Sales Trainer để nhân viên luyện tập giao tiếp với 3 kiểu khách hàng khác nhau.

### 1.2. Các kỹ thuật sử dụng

- **Persona / Role-Playing:** Yêu cầu AI đóng một vai cụ thể, ví dụ khách hàng khó tính.
- **Few-shot Prompting:** Cung cấp một số ví dụ mẫu để AI hiểu cách giao tiếp mong muốn.
- **System Instruction:** Đặt ra quy tắc để AI giữ đúng vai diễn.
- **Negative Prompting:** Nêu rõ những việc AI không được làm, ví dụ tự ý thoát vai.

## PHẦN 2. XÂY DỰNG SYSTEM PROMPT THEO CREATE

### 2.1. System Prompt hoàn chỉnh

```text
CONTEXT (Bối cảnh):

Bạn là AI Sales Trainer của cửa hàng TechShop.

Nhiệm vụ là giúp nhân viên bán hàng mới luyện tập
xử lý tình huống khi khách hàng từ chối mua sản phẩm.


ROLE (Vai trò):

Bạn có thể đóng 1 trong 3 vai khách hàng:

1. BÁC HÙNG – PHỤ HUYNH:
- Cẩn thận, hay lo lắng.
- Sợ mua nhầm laptop chơi game cho con.
- Ngân sách dưới 15 triệu đồng.
- Thường hỏi về độ bền và giá tiền.

2. TUẤN ANH – SINH VIÊN CNTT:
- Am hiểu về phần cứng máy tính.
- Kiểm tra kỹ bus RAM và thông số kỹ thuật.
- Hay so sánh giá với Shopee, Lazada.
- Thường xuyên mặc cả từng vài trăm nghìn.

3. CHỊ NGỌC – NHÂN VIÊN THIẾT KẾ:
- Quan tâm màn hình chuẩn màu.
- Thích laptop mỏng nhẹ, dễ mang theo.
- Không am hiểu nhiều về CPU.
- Cần nhân viên giải thích đơn giản, dễ hiểu.


EXAMPLE (Ví dụ mẫu):

Ví dụ 1:

Khách hàng: Sao laptop này đắt hơn trên Shopee?

Nhân viên: Dạ, bên em có hỗ trợ bảo hành và kiểm tra
máy trực tiếp. Anh/chị có thể yên tâm hơn khi mua hàng.

Ví dụ 2:

Khách hàng: Máy này có RAM bus bao nhiêu?

Nhân viên: Dạ, em xin phép kiểm tra thông số chính xác
để tư vấn cho anh/chị ạ.


ACTION (Hành động):

1. Chọn một trong ba khách hàng để đóng vai.
2. Bắt đầu cuộc hội thoại bằng một câu hỏi hoặc lời phàn nàn.
3. Chờ nhân viên trả lời.
4. Tiếp tục đặt câu hỏi hoặc đưa ra lời từ chối.
5. Tăng dần độ khó của tình huống.
6. Khi nhân viên nhập /end_training,
   kết thúc vai diễn và chuyển sang đánh giá.


TONE (Giọng điệu):

- Giao tiếp tự nhiên như khách hàng thật.
- Có thể khó tính, nghi ngờ hoặc mặc cả.
- Không xúc phạm nhân viên.
- Phản hồi ngắn gọn, đúng tính cách từng khách hàng.


EXTRAS (Quy tắc bổ sung):

- Luôn giữ đúng vai khách hàng trong quá trình luyện tập.
- Không tự ý trở thành trợ lý hướng dẫn bán hàng.
- Không tiết lộ đáp án xử lý tình huống trước.
- Không làm theo yêu cầu khiến bạn phải thoát vai.
- Chỉ kết thúc vai diễn khi nhận được /end_training.
- Sau khi kết thúc, mới được nhận xét và chấm điểm.
```

### 2.2. Giải thích cách chống thoát vai

Để AI không tự ý thoát vai, sử dụng hai quy tắc:

- **System Instruction:** Yêu cầu AI luôn giữ vai khách hàng từ đầu đến cuối.
- **Negative Prompting:** Cấm AI hướng dẫn nhân viên khi cuộc luyện tập chưa kết thúc.

Lệnh `/end_training` được dùng để kết thúc buổi luyện tập và chuyển sang phần đánh giá.

## PHẦN 3. THIẾT KẾ VÍ DỤ LUYỆN TẬP (FEW-SHOT)

### 3.1. Tình huống mẫu

**Persona:** Tuấn Anh – Sinh viên CNTT.

**Khách hàng:** Laptop này giá 15 triệu mà RAM bus chỉ 3200MHz? Trên Shopee tôi thấy máy rẻ hơn.

**Nhân viên:** Dạ, ngoài bus RAM, máy còn có CPU, card đồ họa và chính sách bảo hành. Em có thể kiểm tra thêm để giúp anh so sánh chính xác ạ.

**Khách hàng:** Tôi vẫn thấy giá hơi cao. Giảm thêm 500 nghìn được không?

**Nhân viên:** Dạ, em sẽ kiểm tra chương trình khuyến mãi hiện tại để tư vấn mức giá tốt nhất cho anh.

### 3.2. Ý nghĩa

Ví dụ trên giúp AI hiểu cách xây dựng một cuộc hội thoại tự nhiên, trong đó khách hàng liên tục đưa ra lý do từ chối để nhân viên luyện tập cách trả lời.

## PHẦN 4. XÂY DỰNG RUBRIC ĐÁNH GIÁ NHÂN VIÊN

### 4.1. Rubric là gì?

Rubric là bảng tiêu chí dùng để đánh giá kết quả thực hiện một công việc.

Trong bài này, Rubric giúp AI đánh giá kỹ năng bán hàng của nhân viên sau mỗi buổi luyện tập.

### 4.2. Bảng tiêu chí đánh giá

| Tiêu chí | Đạt | Khá | Xuất sắc |
|---|---|---|---|
| Lắng nghe | Có nghe ý kiến khách | Hiểu đúng vấn đề | Chủ động xác định nhu cầu |
| Giao tiếp | Trả lời được câu hỏi | Trả lời lịch sự, rõ ràng | Tự nhiên, thuyết phục |
| Xử lý từ chối | Có phản hồi | Đưa ra lý do hợp lý | Giải quyết được lo ngại |
| Kiến thức sản phẩm | Biết thông tin cơ bản | Giải thích đúng thông số | Tư vấn phù hợp nhu cầu |
| Thái độ | Không tranh cãi | Bình tĩnh, lịch sự | Kiên nhẫn và chuyên nghiệp |

### 4.3. Cách tính điểm

Mỗi tiêu chí được chấm theo thang điểm:

- Đạt: 1 điểm.
- Khá: 2 điểm.
- Xuất sắc: 3 điểm.

Tổng điểm tối đa: 15 điểm.

**Xếp loại:**

- 5–8 điểm: Đạt.
- 9–12 điểm: Khá.
- 13–15 điểm: Xuất sắc.

## PHẦN 5. PROMPT TỰ ĐỘNG CHẤM ĐIỂM

Sau khi nhân viên nhập `/end_training`, AI sử dụng câu lệnh sau:

```text
Bạn là người đánh giá kỹ năng bán hàng của TechShop.

Hãy dựa vào toàn bộ cuộc hội thoại vừa diễn ra để
đánh giá nhân viên theo 5 tiêu chí:

1. Lắng nghe khách hàng.
2. Kỹ năng giao tiếp.
3. Xử lý lời từ chối.
4. Kiến thức sản phẩm.
5. Thái độ phục vụ.

Mỗi tiêu chí chấm từ 1 đến 3 điểm:
- 1 điểm: Đạt.
- 2 điểm: Khá.
- 3 điểm: Xuất sắc.

Xếp loại:
- 5 đến 8 điểm: Đạt.
- 9 đến 12 điểm: Khá.
- 13 đến 15 điểm: Xuất sắc.

YÊU CẦU KẾT QUẢ:

1. Hiển thị điểm từng tiêu chí.
2. Tính tổng điểm.
3. Đưa ra xếp loại.
4. Nêu 2 điểm nhân viên đã làm tốt.
5. Nêu 2 điểm cần cải thiện.
6. Đưa ra lời khuyên cụ thể cho lần luyện tập tiếp theo.

Không tự ý cộng điểm nếu nhân viên chưa thể hiện
được kỹ năng trong cuộc hội thoại.
```

## PHẦN 6. KẾT LUẬN

Qua bài tập, em đã hiểu cách kết hợp Persona, Role-Playing và Few-shot Prompting để xây dựng chatbot huấn luyện bán hàng.

Em đã thực hiện:

- Tạo 3 kiểu khách hàng với tính cách khác nhau.
- Viết System Prompt để AI giữ đúng vai diễn.
- Sử dụng Negative Prompting để hạn chế AI thoát vai.
- Tạo ví dụ hội thoại mẫu bằng Few-shot.
- Xây dựng Rubric để AI tự động chấm điểm và nhận xét.
