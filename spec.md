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
Với mỗi kịch bản đắt nhất:
- AI nói gì (nguyên văn) · người dùng thấy gì · làm gì tiếp · ai được báo, sau bao lâu.
- Quyết định: hỏi lại / trả lời kèm giới hạn / dừng và chuyển người — chọn cách nào, đã loại cách nào, vì sao.
- Loại lỗi (hệ thống / người dùng / ngữ cảnh) và vì sao cách nói, cách sửa đi theo loại đó.
- 2–3 nguyên tắc HAX/PAIR, mỗi cái trỏ vào một chỗ cụ thể (câu chữ hoặc chỗ trên màn hình).
- Lỗi nào sửa câu chữ không cứu được (nếu có), và thiết kế cứu nó thế nào.

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
