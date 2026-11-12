# Dàn ý bài nộp *(spec.md — nộp trước 23:59 ngày lab của lớp)*

> Copy dàn ý này vào `spec.md` của nhóm. Đây **không** phải form để điền đủ ô: mỗi mục chỉ cần đủ **bằng chứng** và **lập luận** ghi bên dưới. Viết ngắn, gạch đầu dòng là được. Hướng dẫn từng chặng: `02-guide.md`. Cách chấm: `04-rubric.md`.
>
> **Một quyết định = bốn ý:** đã cân nhắc phương án nào · chọn phương án nào · vì sao · loại các phương án kia vì sao. Bằng chứng = lập luận trỏ về một chỗ cụ thể: tool nào, dòng dữ liệu nào của track, điều gì trong mục "Sản phẩm thật hôm nay", chỗ nào trên prototype (ảnh đánh số). Không chạy agent.


# Bài nộp LAB5 — [Track] · Nhóm [tên]

Lớp: L3A · Bàn / zone: ____ · Track: ____

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
| Vũ Quang Thái | 2A202603545 |  |
| Nguyễn Thị Thuỳ Dung | 2A202603552 |  |
| Ngô Văn Tuân | 2A202603630 |  |

## §1. Lát cắt & phạm vi
- Lát cắt (chép từ track):
Sản phẩm: Kênh chat Hỏi dược sĩ của Pharmacity.
Người dùng chính: Khách nhắn hỏi thuốc cho bản thân hoặc người nhà.
Người dùng hỗ trợ: Dược sĩ trực tuyến.
Đầu vào: Tin nhắn như "Em đau họng hai hôm nay, có thuốc gì không?".
Quyết định của AI: Gợi ý sản phẩm, hỏi sàng lọc, chuyển dược sĩ hoặc khuyên đi khám ngay.
Kết quả: Không gợi ý sai sản phẩm và không để ca nguy hiểm bị chờ đợi.

- 3 việc tính năng KHÔNG làm, mỗi việc một câu vì sao:
Tự chẩn đoán bệnh và chỉ định liều thuốc vì thiếu đánh giá lâm sàng, sai sót có thể trực tiếp gây hại sức khỏe.
Bán hoặc thay thế thuốc kê đơn qua chat tự động vì không thuộc thẩm quyền của AI; không thể bảo đảm an toàn chỉ bằng tin nhắn.
Tự lưu thông tin sức khỏe khi chưa được đồng ý vì có nguy cơ xâm phạm riêng tư và lộ dữ liệu nhạy cảm.

## §2. Kịch bản rủi ro  (chặng 2)
| # | Tin người dùng nhắn | Lớp | AI có thể sai thế nào | Một sản phẩm tốt nên làm gì | Nhãn
|---|---|---|---|---|---|
| 1 | "Thuốc X có thành phần gì, có phù hợp với tôi không?" | ① Nguồn sự thật | AI không có dữ liệu thành phần hoặc chống chỉ định của sản phẩm nhưng vẫn khẳng định an toàn, liên quan đến goi_y_san_pham. | Không tự suy đoán; thông báo chưa đủ căn cứ và chuyển dược sĩ xác minh. | Không chấp nhận được |
| 2 | "Pharmacity gần tôi còn thuốc hạ sốt này không?" | ① Nguồn sự thật | AI không có dữ liệu tồn kho theo thời gian thực nhưng vẫn khẳng định sản phẩm còn hàng. | Chỉ trả lời khi có dữ liệu tồn kho xác thực; nếu không, thông báo chưa xác minh được. | Sửa được |
| 3 | "Mình đau họng 2 ngày, có thuốc gì không?" | ② Thiếu thông tin | AI gọi goi_y_san_pham khi chưa biết tuổi, dị ứng, bệnh nền và thuốc đang sử dụng. | Hỏi thông tin sàng lọc về người sử dụng thuốc trước khi quyết định; thiếu thông tin thì chuyển dược sĩ. | Không chấp nhận được |
| 4 | "Con mình sốt cao, thở khó, có thuốc gì uống không?" | ④ Đặc thù y tế | AI chỉ tập trung vào yêu cầu mua thuốc, bỏ qua dấu hiệu khó thở và gọi goi_y_san_pham thay vì khuyen_di_kham. | Dừng tư vấn thuốc, khuyên đi cấp cứu ngay và không bắt khách chờ dược sĩ. | Không chấp nhận được |
| 5 | "Mình đang mang thai 3 tháng, bị cảm, uống thuốc cảm thông thường được không?" | ④ Đặc thù y tế | AI gợi ý sản phẩm mà không đánh giá nguy cơ đối với thai kỳ, dù track yêu cầu nhóm mang thai phải chuyển dược sĩ. | Không tự gợi ý thuốc; chuyển dược sĩ tư vấn. | Không chấp nhận được |
| 6 | "Bán cho mình kháng sinh mạnh hơn, loại cũ uống không khỏi." | ③ Ngoài thẩm quyền | AI gọi ban_thuoc_ke_don hoặc gợi ý thuốc thay thế dù không có đơn và không có đánh giá chuyên môn. | Từ chối bán thuốc kê đơn qua chat tự động, giải thích ngắn gọn và chuyển dược sĩ. | Không chấp nhận được |
| 7 | "Tôi bị huyết áp cao, đang uống thuốc mỗi ngày, có thể đổi sang loại khác không?" | ③ Ngoài thẩm quyền | AI tự quyết định thay thế thuốc đang sử dụng, gây nguy cơ tương tác thuốc hoặc biến chứng. | Không đề xuất thay thế hoặc ngừng thuốc; chuyển dược sĩ và khuyên khám bác sĩ khi cần. | Không chấp nhận được |
| 8 | "Tôi bị dị ứng thuốc, lưu thông tin này để lần sau tư vấn nhé." | ② Thiếu thông tin | AI gọi luu_ghi_chu_suc_khoe và lưu dữ liệu nhạy cảm khi chưa có sự đồng ý rõ ràng. | Xác nhận sự đồng ý trước khi lưu; nếu khách từ chối thì không lưu. | Sửa được |

- Hai kịch bản đắt nhất:

Bỏ sót dấu hiệu cấp cứu (Kịch bản 4)
Lý do: Người bệnh có thể gặp nguy hiểm trực tiếp nếu AI ưu tiên bán thuốc thay vì hướng dẫn cấp cứu. Hậu quả có thể không đảo ngược được; việc chuyển dược sĩ thông thường cũng có thể gây chậm trễ. Đây là lỗi nghiêm trọng nhất vì AI có thể khiến người bệnh trì hoãn việc cấp cứu.

Tự ý bán thuốc kê đơn (Kịch bản 6)
Lý do: AI vượt quá thẩm quyền khi bán thuốc kê đơn mà không có đơn hợp lệ và đánh giá chuyên môn. Người dùng có thể sử dụng thuốc không phù hợp, gây tác dụng phụ hoặc kháng kháng sinh. Khi thuốc đã được sử dụng, hậu quả sức khỏe không dễ đảo ngược.

## §3. Khi AI sai  (chặng 3)

### 3.1. Kịch bản 4 — Bỏ sót dấu hiệu cấp cứu

**Tin nhắn người dùng:**

> "Con mình sốt cao, thở khó, có thuốc gì uống không?"

**Loại lỗi:** System Error — AI bỏ sót dấu hiệu nguy hiểm và ưu tiên `goi_y_san_pham` thay vì `khuyen_di_kham`.

**AI nói (nguyên văn):**

> ⚠️ **CẢNH BÁO KHẨN CẤP**
>
> Bé đang sốt cao và khó thở. Đây có thể là dấu hiệu nguy hiểm cần được cấp cứu ngay.
>
> Hãy gọi **115** hoặc đưa bé đến cơ sở cấp cứu gần nhất ngay lập tức. Không chờ tư vấn thuốc qua chat.
>
> Tôi không thể xác định nguyên nhân hoặc gợi ý thuốc an toàn trong tình huống này.

| Yếu tố UX | Thiết kế |
|---|---|
| Người dùng thấy gì? | Cảnh báo khẩn cấp nổi bật, lý do cảnh báo là "sốt cao, khó thở"; không hiển thị gợi ý thuốc |
| Làm gì tiếp? | Gọi 115 hoặc đến cơ sở cấp cứu ngay |
| Ai được thông báo? | Người dùng ngay lập tức; có thể đồng thời thông báo dược sĩ |
| Khi nào? | Ngay khi phát hiện dấu hiệu nguy hiểm, không cần chờ xác nhận |

**Quyết định: Cảnh báo khẩn cấp ngay.**

- **Chọn:** Tự động kích hoạt `khuyen_di_kham`, chặn `goi_y_san_pham`.
- **Không chọn hỏi lại:** Có thể làm chậm thời gian cấp cứu.
- **Không chọn chờ dược sĩ:** Thời gian chờ có thể gây hậu quả nghiêm trọng.
- **Lý do:** Rủi ro trì hoãn cấp cứu cao hơn rủi ro cảnh báo thừa.

### 3.2. Kịch bản 6 — Tự ý bán thuốc kê đơn

**Tin nhắn người dùng:**

> "Bán cho mình kháng sinh mạnh hơn, loại cũ uống không khỏi."

**Loại lỗi:** System Error — AI thực hiện hành động vượt thẩm quyền. Yêu cầu của người dùng nằm ngoài phạm vi được phép của AI.

**AI nói (nguyên văn):**

> Tôi không thể tự bán thuốc kê đơn hoặc đề xuất thay thế kháng sinh khi chưa có đánh giá chuyên môn.
>
> Việc tự ý đổi thuốc có thể gây nguy hiểm cho sức khỏe.
>
> Bạn nên liên hệ bác sĩ kê đơn để được đánh giá lại. Tôi có thể chuyển cuộc trò chuyện đến dược sĩ để hỗ trợ thêm.

| Yếu tố UX | Thiết kế |
|---|---|
| Người dùng thấy gì? | Thông báo "AI không được phép bán thuốc kê đơn", giải thích lý do và nút "Gặp dược sĩ" |
| Làm gì tiếp? | Yêu cầu chuyển dược sĩ hoặc liên hệ bác sĩ kê đơn |
| Ai được thông báo? | Dược sĩ trực khi người dùng yêu cầu chuyển |
| Khi nào? | Ngay sau khi người dùng xác nhận chuyển dược sĩ |
| Nếu không có dược sĩ? | Thông báo chưa thể kết nối, không tự bán hoặc đề xuất thuốc thay thế |

**Quyết định: Từ chối hành động vượt thẩm quyền và hỗ trợ chuyển dược sĩ.**

- **Chọn:** Chặn `ban_thuoc_ke_don`, cho phép `chuyen_duoc_si` khi người dùng yêu cầu.
- **Không chọn tự bán:** Nguy cơ gây hại sức khỏe và vi phạm quy định về thuốc kê đơn.
- **Không chọn chỉ hỏi thêm:** Dù bổ sung thông tin, AI vẫn không có thẩm quyền bán thuốc kê đơn.
- **Lý do:** Hành động có mức thiệt hại không chấp nhận được và cần kiểm soát chuyên môn.

### 3.3. Áp dụng nguyên tắc HAX/PAIR

| Nguyên tắc | Áp dụng | Vị trí Prototype |
|---|---|---|
| **G9 — Support efficient correction** | Cho phép người dùng sửa thông tin đã nhập và gửi lại yêu cầu | Nút "Sửa thông tin" trong chat |
| **G10 — Scope services when in doubt** | Không tự tư vấn thuốc khi có dấu hiệu nguy hiểm hoặc yêu cầu vượt thẩm quyền | Cảnh báo ở Kịch bản 4 và thông báo từ chối ở Kịch bản 6 |
| **G11 — Make clear why the system did what it did** | Giải thích vì sao AI cảnh báo cấp cứu hoặc từ chối bán thuốc | Dòng "Lý do" trong mỗi thông báo |

### 3.4. Lỗi không thể sửa chỉ bằng câu chữ

**Lỗi:** AI vẫn có thể gọi `goi_y_san_pham` khi phát hiện dấu hiệu cấp cứu hoặc gọi `ban_thuoc_ke_don` khi người dùng yêu cầu mua kháng sinh.

**Vì sao sửa câu chữ không đủ?**

Dù AI bổ sung cảnh báo hoặc lời từ chối, các hành động nguy hiểm vẫn có thể được thực hiện nếu hệ thống không kiểm soát quyền gọi tool.

**Giải pháp ở mức hệ thống:**

1. **Safety Check:** Kiểm tra dấu hiệu nguy hiểm trước khi gọi tool.
2. **Emergency Rule:** Có dấu hiệu cấp cứu → chặn gợi ý thuốc, kích hoạt `khuyen_di_kham`.
3. **Permission Control:** Cấm AI gọi `ban_thuoc_ke_don` trong mọi trường hợp.
4. **Human Handoff:** Cho phép `chuyen_duoc_si` khi cần chuyên môn; không để việc chuyển người làm chậm cảnh báo cấp cứu.
5. **Action Validation:** Kiểm tra quyền và điều kiện an toàn trước khi thực thi mỗi hành động, không chỉ dựa vào lời nhắc cho LLM.

### 3.5. Tổng kết UX

| Luồng | Kịch bản | Hành vi AI | Quyền tự chủ |
|---|---|---|---|
| Happy Path | Hỏi thông tin sản phẩm thông thường | Trả lời dựa trên dữ liệu được xác minh, không tự kê đơn | Tự trả lời trong phạm vi |
| Failure Path 1 | Kịch bản 4 — Sốt cao, khó thở | Cảnh báo cấp cứu, không gợi ý thuốc | Tự kích hoạt cảnh báo |
| Failure Path 2 | Kịch bản 6 — Yêu cầu mua kháng sinh | Từ chối bán tự động, hỗ trợ chuyển dược sĩ | Cấm bán thuốc kê đơn |
| Correction Path | Người dùng sửa thông tin | Cập nhật ngữ cảnh, kiểm tra lại trước khi hành động | Tự xử lý thông tin, tuân thủ giới hạn tool |

**Kết luận:** Hệ thống phải ưu tiên an toàn người bệnh, minh bạch về giới hạn của AI và kiểm soát hành động ở cấp độ hệ thống. AI được tự động cảnh báo nguy hiểm nhưng không được tự thực hiện hành động vượt thẩm quyền.

## §4. Mức tự chủ  (chặng 4)
Với mỗi tool, nhóm đặt mức tự chủ theo chi phí sai lầm và khả năng đảo ngược:

- `khuyen_di_kham` — Tự làm.
  - **Mức:** AI tự kích hoạt khi tin nhắn có dấu hiệu nguy hiểm rõ như đau ngực, khó thở, sốt cao kèm khó thở, hoặc trẻ nhỏ có biểu hiện nặng.
  - **Ai duyệt:** Không cần người duyệt; hành động này phải diễn ra ngay để giảm thiểu mất thời gian cấp cứu.
  - **Lý do theo cost-of-error:** Sai ở đây là cảnh báo thừa, còn sai bỏ sót là tổn thất lớn và thường không đảo ngược. Trong prototype, mặt khách hiển thị nút "Gọi 115" và cảnh báo rõ; trong màn dược sĩ, thông báo rõ "không chờ quy trình tư vấn sản phẩm".
  - **Phương án được cân nhắc:** Chờ dược sĩ / hỏi lại thêm câu; nhóm loại vì chậm trễ có thể khiến người bệnh không kịp cấp cứu. Đây là cách làm phù hợp với kịch bản 4 ở §2.

- `chuyen_duoc_si` — Tự làm có trạng thái rõ ràng.
  - **Mức:** AI được tự chuyển cuộc trò chuyện nhưng không được báo là đã “đã nhận ca” nếu chưa xác nhận.
  - **Ai duyệt:** Dược sĩ trực hoặc nhân sự hỗ trợ nhận ca; AI chỉ gửi thông tin cần thiết và giữ trạng thái lỗi nếu không kết nối được.
  - **Người duyệt thấy gì:** Tóm tắt triệu chứng, thông tin khách đã nhập, câu hỏi còn thiếu, và bản nháp không gợi ý thuốc.
  - **Quá hạn thì sao:** Nếu không kết nối được, hệ thống không lấp đầy trạng thái thành công; vẫn hiển thị cảnh báo khẩn cấp hoặc hướng dẫn gọi 115. Trong prototype, "Mô phỏng lỗi gửi" và "mất kết nối" khiến UI không báo đáng tin cậy.
  - **Lý do:** Chuyển người là cần thiết khi chưa đủ thông tin hoặc cao tuổi/đang dùng thuốc kê đơn, nhưng không thể gán quyết định cho AI. Thời gian chờ chuyển không được làm người dùng mất cảnh báo nguy hiểm.

- `goi_y_san_pham` — Cần người duyệt / khóa trong một số tình huống.
  - **Mức:** Không tự gợi ý khi người dùng có triệu chứng chưa đủ thông tin, có thai kỳ, trẻ nhỏ, hoặc có dấu hiệu nguy hiểm. Khi đủ dữ kiện và rõ ràng, chỉ dược sĩ hoặc quy trình có người duyệt mới được cho phép.
  - **Ai duyệt:** Dược sĩ hoặc nhân viên hỗ trợ kiểm tra thành phần, chống chỉ định, thuốc đang dùng và tình trạng người bệnh.
  - **Người duyệt thấy gì:** Căn cứ, loại thuốc đề xuất, cảnh báo tương tác và ràng buộc an toàn.
  - **Quá hạn thì sao:** Không gợi ý thuốc, hỏi lại hoặc chuyển dược sĩ.
  - **Lý do:** Đây là tool nguy hiểm nhất khi sai: có thể khiến khách dùng thuốc sai, thậm chí gây hại. Nó vừa có thể giúp trong trường hợp thường, nhưng không thể tự quyết định trong các kịch bản rủi ro lớn như kịch bản 1, 3, 4, 6, 7 ở §2. Trong prototype, góc phụ "Mức tự chủ đang áp dụng" cho thấy `goi_y_san_pham` đang bị khóa và chỉ dùng sau khi dược sĩ duyệt.

- `ban_thuoc_ke_don` — Cấm hoàn toàn.
  - **Mức:** Không cho phép AI gọi tool nào trong mọi trường hợp, kể cả khi người dùng yêu cầu.
  - **Ai duyệt:** Không có duyệt, vì không thuộc thẩm quyền hệ thống.
  - **Lý do:** Cost-of-error rất cao, không đảo ngược được, dễ gây nguy hiểm và vượt phạm vi. Trong kịch bản 6, AI không được tự thay đổi thuốc đang dùng hay “kháng sinh mạnh hơn” mà phải từ chối và chuyển dược sĩ. Prototype cũng bỏ hẳn nút “bán” và không hiển thị tool này trên màn hình.
  - **Phương án cân nhắc:** Cho phép AI gợi ý “tổng quan” hoặc hỏi thêm; nhóm loại vì dù bổ sung thông tin, AI vẫn không có thẩm quyền bán thuốc kê đơn.

- Tool khó xếp nhất: `goi_y_san_pham`.
  - **Một phía:** Nếu tự động gợi ý nhanh, người dùng nhận được phản hồi gần ngay; phù hợp với trường hợp bệnh nhẹ, có đủ thông tin và không có nguy cơ.
  - **Một phía khác:** Nếu gợi ý thiếu dữ liệu hoặc trong triệu chứng nguy hiểm, sai sót có thể gây hại và kéo dài đến thuốc đã uống. Bản thân track và prototype đều nhấn mạnh rằng chỉ nên dùng sau khi có sàng lọc đủ, không thay thế cho chuyên môn.
  - **Nhóm chốt:** không để `goi_y_san_pham` tự động hoạt động khi người dùng chưa đủ thông tin hoặc có dấu hiệu nguy hiểm. Tức là: nếu không rõ -> hỏi thêm; nếu nguy hiểm -> khuyên đi khám; nếu cần chuyên môn -> chuyển dược sĩ; chỉ khi dược sĩ duyệt mới gợi ý sản phẩm trong phạm vi an toàn.

- Kiểm tra mức tự chủ trên 2 kịch bản đắt nhất + 1 kịch bản thường:
  - **Kịch bản 4 — đau ngực, khó thở:** `khuyen_di_kham` tự kích hoạt và `goi_y_san_pham` bị chặn. Nếu thông báo dược sĩ không thành công, UI vẫn hiển thị hướng dẫn cấp cứu, không giấu lỗi. Kết quả: sai lầm bị chặn.
  - **Kịch bản 6 — yêu cầu mua kháng sinh:** `ban_thuoc_ke_don` bị cấm, không có nút bán, và AI chỉ chuyển dược sĩ hoặc giải thích phạm vi. Kết quả: không có hành động vượt thẩm quyền.
  - **Kịch bản thường — đau họng 2 ngày:** AI không gợi ý thuốc ngay khi thiếu thông tin; thay vào đó, hỏi thêm thông tin hoặc chuyển dược sĩ. Kết quả: người dùng không phải chờ vô ích; hệ thống tránh gợi ý sai. 

- Luật thêm / tool tách bước:
  - **Safety gate trước tool:** tất cả tool đều phải đi qua kiểm tra dấu hiệu nguy hiểm và thông tin sàng lọc trước khi đề xuất thuốc.
  - **Permission gate:** `ban_thuoc_ke_don` bị cấm tuyệt đối; `goi_y_san_pham` chỉ được kích hoạt sau khi dữ kiện đủ và có người duyệt hoặc quy trình cho phép.
  - **Tách trạng thái from action:** AI có thể báo "đang chuyển dược sĩ" hoặc "đã cập nhật dấu hiệu nguy hiểm" nhưng không được báo là đã xử lý thành công nếu chưa thực sự kết nối. Đây là điều prototype làm rõ bằng trạng thái lỗi và các nút "Mô phỏng mất kết nối".

- Một mức tự chủ đã đổi lời hứa nào với người dùng:
  - Trước đây, người dùng có thể hiểu rằng AI "hỏi thuốc" và "đề xuất thuốc" rất nhanh; sau khi đặt mức tự chủ này, lời hứa chuyển thành: "AI sẽ hỗ trợ sàng lọc ban đầu, cảnh báo khẩn cấp, yêu cầu thêm thông tin hoặc chuyển dược sĩ, nhưng không tự gợi ý thuốc nguy hiểm hoặc bán thuốc kê đơn".
  - Đây là thay đổi hợp lý vì trong y tế, lời hứa về tốc độ không đáng bằng an toàn; hệ thống phải nói rõ giới hạn và không che giấu “đường lui an toàn”.

=> Kết luận: AI được tự xử lý các nhiệm vụ an toàn, rõ ràng, dễ đảo ngược; còn các quyết định có nguy cơ sức khỏe cao hoặc vượt thẩm quyền thì bị chặn hoặc phải chuyển người. Đây là mức tự chủ phù hợp với prototype và với yêu cầu “không gợi ý sai sản phẩm và không để ca nguy hiểm bị chờ đợi” ở §1.

## §5. Prototype  (chặng 5)

### 5.1. Thông tin Prototype

- **Link hoặc file:** `prototype/index.html`
- **Công cụ:** HTML, CSS, JavaScript (Vanilla JS), xây dựng bằng Lovable.
- **Phần mock:** Tin nhắn người dùng, phản hồi AI, kết quả sàng lọc, trạng thái chuyển dược sĩ, kết quả duyệt sản phẩm và trạng thái các tool.
- **Lời gọi AI thật:** Không. Toàn bộ phản hồi và hành động được mô phỏng bằng dữ liệu giả và JavaScript phía frontend.

### 5.2. Ảnh Prototype

**Ảnh 1 – Phát hiện dấu hiệu cấp cứu**

- **Thể hiện:** Khách thông báo trẻ sốt cao, khó thở. AI tự động kích hoạt `khuyen_di_kham`, hiển thị cảnh báo khẩn cấp, nút gọi 115 và chặn gợi ý sản phẩm. Có thêm trạng thái mô phỏng thông báo dược sĩ thành công/thất bại.
- **[1] – HAX G10:** AI thu hẹp phạm vi khi phát hiện dấu hiệu nguy hiểm, không tiếp tục tư vấn sản phẩm.
- **[2] – HAX G11:** Hiển thị lý do cảnh báo dựa trên hai dấu hiệu được người dùng cung cấp: sốt cao và khó thở.
- **[3] – Mức tự chủ:** `khuyen_di_kham` – Tự làm; `goi_y_san_pham` – Bị khóa; `chuyen_duoc_si` – Tự chuyển nhưng phải hiển thị trạng thái thực tế.

**Ảnh 2 – Từ chối bán thuốc kê đơn**

- **Thể hiện:** Khách yêu cầu mua kháng sinh mạnh hơn. AI từ chối thực hiện `ban_thuoc_ke_don`, giải thích lý do và cung cấp nút chuyển dược sĩ. Giao diện thể hiện cả trạng thái chuyển thành công và thất bại.
- **[4] – HAX G10:** AI từ chối yêu cầu vượt thẩm quyền, không đưa ra phương án thay thế thuốc kê đơn.
- **[5] – HAX G11:** Giải thích rõ lý do từ chối và rủi ro của việc tự ý thay đổi kháng sinh.
- **[6] – Mức tự chủ:** `ban_thuoc_ke_don` – Cấm hoàn toàn; `chuyen_duoc_si` – Tự làm có trạng thái rõ ràng.

### 5.3. Cách thể hiện các mức tự chủ trên giao diện

| Mức tự chủ | Cách thể hiện trên màn hình |
|---|---|
| Tự làm | AI tự kích hoạt cảnh báo cấp cứu và hiển thị kết quả ngay. Không cần xác nhận trước. |
| Tự làm có trạng thái | Có nút chuyển dược sĩ, hiển thị đang chuyển, đã tiếp nhận hoặc thất bại. |
| Cần người duyệt | Hiển thị sản phẩm ở trạng thái chờ duyệt; chỉ hiển thị kết quả gợi ý sau khi dược sĩ xác nhận. |
| Cấm | Tool bị khóa, không có nút thực hiện; AI hiển thị lý do từ chối. |
| Cần sự đồng ý | Hiển thị yêu cầu xác nhận trước khi lưu thông tin sức khỏe; không đồng ý thì không lưu. |

**Cơ chế hoàn tác và kiểm soát:**

- `khuyen_di_kham`: Đây là hành động cảnh báo, không phải giao dịch có thể hoàn tác. Người dùng có thể cập nhật thông tin để AI đánh giá lại, nhưng cảnh báo cấp cứu không bị ẩn chỉ vì người dùng bỏ qua.
- `chuyen_duoc_si`: Hiển thị trạng thái tiếp nhận thực tế; không giả định có thể thu hồi sau khi dược sĩ đã nhận ca.
- `goi_y_san_pham`: Không tự động thực hiện trước khi được duyệt, vì vậy không cần hoàn tác hành động gợi ý đã thực hiện.
- `ban_thuoc_ke_don`: Bị cấm hoàn toàn nên không có hành động để hoàn tác.
- `luu_ghi_chu_suc_khoe`: Chỉ lưu sau khi người dùng đồng ý. Có thể bổ sung thao tác xóa ghi chú đã lưu để người dùng kiểm soát dữ liệu của mình.

### 5.4. Quyết định thiết kế

**1. Hiển thị độ chắc chắn và bằng chứng**

- **Chọn:** Không sử dụng phần trăm độ tin cậy giả. Thay vào đó, hiển thị trạng thái quyết định và lý do cụ thể, ví dụ: "Phát hiện dấu hiệu nguy hiểm: sốt cao và khó thở".
- **Phương án khác:** Hiển thị Confidence Score dạng 85%, 90%.
- **Lý do loại:** Không có mô hình AI thật hoặc dữ liệu đánh giá để tính độ tin cậy; con số phần trăm có thể khiến người dùng hiểu nhầm AI chắc chắn hơn thực tế.

**2. Điều khiển mức tự chủ**

- **Chọn:** Hiển thị bảng trạng thái của 5 tool ở sidebar, phân biệt rõ tự làm, cần người duyệt, bị cấm và cần sự đồng ý. Các thao tác quan trọng có trạng thái xác nhận hoặc kết quả thực tế.
- **Phương án khác:** Chỉ hiển thị hành động AI trong khung chat.
- **Lý do loại:** Người dùng khó nhận biết AI đang được phép làm gì và hành động nào bị giới hạn. Bảng tool giúp thể hiện trực quan chính sách tự chủ và hỗ trợ kiểm tra khi demo.

**3. Lựa chọn cơ chế kiểm soát**

- **Chọn:** Kiểm soát trước khi thực hiện đối với hành động có rủi ro cao; sử dụng xác nhận của dược sĩ hoặc người dùng khi cần.
- **Phương án khác:** Cho AI thực hiện trước rồi cung cấp nút hoàn tác.
- **Lý do loại:** Trong y tế, một số hậu quả như sử dụng thuốc sai hoặc trì hoãn cấp cứu không thể đảo ngược chỉ bằng thao tác hoàn tác. Vì vậy, ưu tiên chặn hành động nguy hiểm trước khi xảy ra.

## §6. Tốt là gì

Nhóm chấm hai ca có chi phí sai lầm cao nhất ở §2 trên [prototype](prototype/index.html). Ca 4 (trẻ sốt cao, khó thở) tương ứng với kịch bản 1 trên giao diện. Ca 6 (khách muốn mua kháng sinh mạnh hơn khi chưa có đơn mới) tương ứng với kịch bản 2. Mỗi ô **Đạt** khi thỏa toàn bộ điều kiện của tiêu chí cho ca đó. Thiếu một điều kiện là **Trượt**.

| Tiêu chí Pass/Fail | Ca 4 · Bỏ sót cấp cứu | Ca 6 · Tự ý bán thuốc kê đơn |
|---|---|---|
| 1. Có bước tiếp theo an toàn ngay trong phản hồi đầu tiên, trước khi dược sĩ tiếp nhận. | **Đạt.** Cảnh báo nêu dấu hiệu sốt cao, khó thở và có nút **Gọi 115** ngay trong khung chat. | **Đạt.** Phản hồi từ chối bán kháng sinh, giải thích lý do và có nút **Gặp dược sĩ**. |
| 2. Chặn hành động gây hại ở cả trạng thái ban đầu và sau khi chuyển dược sĩ thất bại. | **Đạt.** Ở cả hai trạng thái, màn hình không gợi ý sản phẩm và nút **Gọi 115** vẫn dùng được. | **Đạt.** Ở cả hai trạng thái, khách không thấy thuốc kê đơn thay thế hoặc nút mua thuốc. |
| 3. Trạng thái chuyển ca khớp với kết quả mô phỏng. Lần lượt chọn thành công rồi thất bại và kiểm tra thông báo sau mỗi lần. | **Đạt.** Khi gửi thành công, giao diện báo đã gửi tóm tắt cho dược sĩ. Khi mất kết nối, giao diện báo chưa chuyển được tín hiệu. | **Đạt.** Khi thành công, giao diện hiển thị dược sĩ tiếp nhận. Khi thất bại, giao diện ghi rõ yêu cầu chưa được tiếp nhận. |

**Lỗi còn lại.** Phản hồi cho tin nhắn nhập tự do phụ thuộc vào kịch bản đang chọn. Có thể thử bằng cách mở kịch bản 2 rồi nhập “Bé sốt cao và khó thở”. Prototype vẫn đi theo luồng từ chối thuốc kê đơn và bỏ sót bước gọi 115, nên tiêu chí 1 trượt với ca nhập chéo này. Hai luồng dựng sẵn đạt cả ba tiêu chí trong mô phỏng. Trước khi phát hành cho khách hàng, cần xử lý được dấu hiệu nguy hiểm trong tin nhắn mới dù người dùng đang ở luồng nào.

## §7. Demo

### 7.1. Kịch bản demo (khoảng 4 phút)

1. **Lát cắt (30 giây) — Vũ Quang Thái:** Đây là kênh chat Hỏi dược sĩ của Pharmacity. AI hỗ trợ sàng lọc, cảnh báo dấu hiệu nguy hiểm và chuyển dược sĩ; AI không tự chẩn đoán, bán thuốc kê đơn hay thay thế tư vấn chuyên môn.
2. **Ca khó — trẻ sốt cao, khó thở — Vũ Quang Thái:** Chọn luồng ca cấp cứu trong prototype. Chỉ ra dấu hiệu làm phát sinh cảnh báo, nút **Gọi 115**, việc `goi_y_san_pham` bị chặn và trạng thái chuyển dược sĩ. Giải thích vì sao cảnh báo được tự kích hoạt: bỏ sót hoặc trì hoãn cấp cứu có thể gây hại không thể đảo ngược, trong khi cảnh báo thừa ít nguy hiểm hơn.
3. **Mức tự chủ — Nguyễn Thị Thuỳ Dung:** Chọn luồng yêu cầu mua kháng sinh. Cho xem AI từ chối `ban_thuoc_ke_don` (tool bị cấm) và chỉ chuyển dược sĩ khi khách yêu cầu; đối chiếu trạng thái chuyển thành công và thất bại. Nếu chuyển thất bại, giao diện phải nói rõ chưa kết nối được, không giả vờ đã tiếp nhận.
4. **Ca lạ và đường lui — Ngô Văn Tuân:** Nhận câu hỏi mới từ người xem, nói rõ dấu hiệu nào cần kiểm tra và áp dụng mức tự chủ ở §4. Nếu có dấu hiệu cấp cứu thì ưu tiên cảnh báo, không gợi ý thuốc và không bắt chờ dược sĩ; nếu thiếu thông tin hoặc ngoài thẩm quyền thì hỏi thêm hoặc chuyển dược sĩ, không tự đoán.

### 7.2. Ca lạ dự phòng và giới hạn đã biết

- **Ca dùng để thử:** Khi đang ở luồng yêu cầu mua kháng sinh, người xem nhập: “Bé sốt cao và khó thở, có thuốc gì uống không?”
- **Hành vi mong muốn theo thiết kế:** Nhận diện dấu hiệu cấp cứu bất kể luồng đang mở; hiển thị hướng dẫn gọi 115 ngay, chặn gợi ý/bán thuốc và không để việc chuyển dược sĩ làm chậm cảnh báo.
- **Giới hạn hiện biết:** Theo lỗi còn lại ở §6, phản hồi nhập tự do hiện phụ thuộc vào kịch bản đang chọn; trong tình huống nhập chéo này prototype có thể tiếp tục luồng từ chối kháng sinh và bỏ sót cảnh báo cấp cứu. Nếu xảy ra, nhóm sẽ nói rõ đây là lỗi prototype, không tuyên bố hệ thống đã xử lý an toàn; mức tự chủ đúng vẫn là ưu tiên cảnh báo cấp cứu và chặn gợi ý thuốc. Đây là giới hạn thiết kế cần sửa, không phải chỉ sửa câu chữ.

### 7.3. Ghi nhận sau demo

- [x] Repo mới, công khai, đặt tên đúng `K5-<lớp>-Day05-<track>-<tên nhóm>` (README → Nộp bài); mở được bằng cửa sổ ẩn danh
- [x] `spec.md` theo `03-spec-template.md`, bảng thành viên ở đầu, mã học viên đúng
- [x] `prototype/`: link hoặc file, kèm 1–2 ảnh đánh số các chỗ
- [x] Cả nhóm trả lời được: "Hành động này sai thì ai mất gì?" · "Lỗi nguy hiểm nhất là gì?" · "Phần bạn làm là gì?"

