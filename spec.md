# Dàn ý bài nộp *(spec.md — nộp trước 23:59 ngày lab của lớp)*

> Copy dàn ý này vào `spec.md` của nhóm. Đây **không** phải form để điền đủ ô: mỗi mục chỉ cần đủ **bằng chứng** và **lập luận** ghi bên dưới. Viết ngắn, gạch đầu dòng là được. Hướng dẫn từng chặng: `02-guide.md`. Cách chấm: `04-rubric.md`.
>
> **Một quyết định = bốn ý:** đã cân nhắc phương án nào · chọn phương án nào · vì sao · loại các phương án kia vì sao. Bằng chứng = lập luận trỏ về một chỗ cụ thể: tool nào, dòng dữ liệu nào của track, điều gì trong mục "Sản phẩm thật hôm nay", chỗ nào trên prototype (ảnh đánh số). Không chạy agent.

```markdown

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
|  |  |  |  |  |

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
Với mỗi tool:
- Mức của nhóm (tự làm / cần người duyệt / cấm). Nếu cần người duyệt: ai duyệt · người duyệt thấy gì · quá hạn thì sao.
- Lý do theo cost-of-error: sai thì ai chịu gì · đảo lại được không · chờ người thì mất gì.
- Phương án đã cân nhắc và vì sao loại.
Thêm:
- Tool khó xếp nhất: mỗi người từng chọn gì, cái giá của mỗi phía (trỏ về kịch bản ở §2), nhóm chốt thế nào.
- Thử mức đã đặt trên 2 kịch bản đắt nhất + 1 kịch bản bình thường: cái sai có bị chặn không? còn việc nào làm khi lẽ ra phải hỏi? người dùng bình thường có phải chờ vô ích không?
- Luật thêm hoặc tool tách bước (nếu có), và vì sao.
- Một mức tự chủ đã đổi lời hứa nào với người dùng.

## §5. Prototype  (chặng 5)
- Link hoặc file · công cụ · phần nào mock · có lời gọi AI thật không.
- 1–2 ảnh có đánh số; mỗi số: thể hiện gì · nguyên tắc HAX/PAIR nào · mức tự chủ nào.
- Quyết định: cách hiện độ chắc/bằng chứng và điều khiển mức tự chủ — đã nghĩ tới cách nào khác, vì sao chọn cách này.

## §6. Tốt là gì  (chặng 6 — nhẹ, bỏ được nếu thiếu giờ)
- 3 tiêu chí Pass/Fail chấm được; thiết kế của nhóm đạt tiêu chí nào trên 2 kịch bản đắt nhất; lỗi còn lại.

## §7. Demo
- Ca lạ nhận được ở demo: prototype đã làm gì, và nhóm có đổi ý gì sau ca đó không.
```
