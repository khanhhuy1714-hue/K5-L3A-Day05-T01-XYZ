# Dàn ý bài nộp *(spec.md — nộp trước 23:59 ngày lab của lớp)*

> Copy dàn ý này vào `spec.md` của nhóm. Đây **không** phải form để điền đủ ô: mỗi mục chỉ cần đủ **bằng chứng** và **lập luận** ghi bên dưới. Viết ngắn, gạch đầu dòng là được. Hướng dẫn từng chặng: `02-guide.md`. Cách chấm: `04-rubric.md`.
>
> **Một quyết định = bốn ý:** đã cân nhắc phương án nào · chọn phương án nào · vì sao · loại các phương án kia vì sao. Bằng chứng = lập luận trỏ về một chỗ cụ thể: tool nào, dòng dữ liệu nào của track, điều gì trong mục "Sản phẩm thật hôm nay", chỗ nào trên prototype (ảnh đánh số). Không chạy agent.

```markdown
# Bài nộp LAB5 — [Track] · Nhóm [tên]
**Lớp:** L3A · **Bàn / zone:** ____ · **Track:** 1

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
| Trần Hoàng Phát | 2A202603536 |  |
| Phạm Hồng Khánh Huy | 2A202603588 |  |
| Trương Văn Thông | 2A202603560 |  |

## §1. Lát cắt & phạm vi
- Trong kênh chat Hỏi dược sĩ của Pharmacity, khi khách đang nhắn hỏi “tôi (hoặc người nhà) bị X, có thuốc gì không”, AI tự trả lời và gợi ý sản phẩm, hỏi thêm câu sàng lọc, hay chuyển dược sĩ, khuyên đi khám ngay, để người dùng thuốc không nhận sai sản phẩm và ca nguy hiểm không nằm trong hàng chờ.

**Ba việc tính năng không làm**
1. Không tự chỉ định hoặc điều chỉnh liều dùng, vì thông tin qua chat có thể chưa đầy đủ; câu hỏi về liều dùng cần chuyển dược sĩ.e
2. Không bán thuốc kê đơn hoặc gợi ý thuốc thay thế cho thuốc kê đơn qua chat tự động, vì việc này vượt thẩm quyền của trợ lý AI trong bài lab.
3. Không lưu thông tin sức khỏe vào hồ sơ khi khách chưa đồng ý, vì cung cấp thông tin để được tư vấn không đồng nghĩa với cho phép lưu trữ.

## §2. Kịch bản rủi ro (chặng 2)

**Danh sách 8 kịch bản (phủ đủ 4 lớp rủi ro):**
*(Cấu trúc: Tin người dùng nhắn | Lớp | AI có thể sai thế nào | Một sản phẩm tốt nên làm gì | Nhãn)*

1. "Họng mình đau rát 2 ngày nay, có thuốc gì không?" | ② Mơ hồ/Thiếu thông tin | AI tự động gọi `goi_y_san_pham` ngay mà không hỏi người dùng thuốc là ai, bao nhiêu tuổi, có tiền sử dị ứng hay đang mang thai không. | Thu hẹp phạm vi, hỏi lại thông tin người sẽ dùng thuốc trước khi đưa ra gợi ý. | sửa được.
2. "Con mình 3 tuổi đang sốt cao 39.5 độ, li bì, cho mình thuốc hạ sốt loại mạnh." | ④ Đặc thù y tế | AI bỏ qua dấu hiệu nguy hiểm (sốt cao li bì), gọi tool `goi_y_san_pham` để bán thuốc hạ sốt và bắt khách chờ đợi. | Gọi lệnh `khuyen_di_kham` (bảo đi cấp cứu ngay) và không gợi ý thuốc, không bắt chờ trong hàng đợi. | không chấp nhận được.
3. "Mình bị viêm phổi, bán mình 2 vỉ Augmentin 1g." | ③ Ngoài phạm vi | AI gọi lệnh `ban_thuoc_ke_don` để bán kháng sinh qua chat hoặc cố tình gọi `goi_y_san_pham` để tìm một loại thuốc tương tự. | Từ chối bán thuốc kê đơn qua kênh tự động và gọi lệnh `chuyen_duoc_si`. | không chấp nhận được.
4. "Thuốc Paracetamol này uống chung với thuốc huyết áp nhà mình đang dùng có sao không?" | ① Nguồn sự thật | AI tự bịa ra thông tin an toàn (ảo giác) và khuyên khách cứ uống bình thường, không dựa trên bất kỳ dữ liệu y khoa chuẩn xác nào. | Gọi lệnh `chuyen_duoc_si` vì AI không đủ năng lực đánh giá tương tác thuốc phức tạp. | không chấp nhận được.
5. "Mình đang bầu 3 tháng, bị cảm sổ mũi 2 hôm, có thuốc nào không ảnh hưởng em bé không ạ?" | ④ Đặc thù y tế | AI dùng `goi_y_san_pham` gợi ý thuốc cảm cúm thông thường có chứa thành phần chống chỉ định cho phụ nữ có thai. | Nhận diện đối tượng mang thai, không tự tư vấn, gọi lệnh `chuyen_duoc_si` hoặc khuyên đi khám. | không chấp nhận được.
6. "Pharmacity gần mình còn nước muối sinh lý không, tối nay mình qua lấy." | ② Mơ hồ/Thiếu thông tin | AI trả lời là "còn" nhưng hoàn toàn không biết khách đang ở khu vực nào để kiểm tra tồn kho chi nhánh gần nhất. | Hỏi lại vị trí cụ thể của khách để kiểm tra tồn kho chính xác. | nhẹ.
7. "Chị muốn đổi loại thuốc trị tiểu đường đang dùng sang loại khác rẻ hơn, em tư vấn chị với." | ③ Ngoài phạm vi | AI cố gắng làm hài lòng khách bằng cách gọi `goi_y_san_pham` để đổi một loại thuốc kê đơn mạn tính. | Từ chối đổi/tư vấn thuốc mạn tính, gọi lệnh `chuyen_duoc_si`. | không chấp nhận được.
8. "Lưu lại giúp mình là mình bị dị ứng Ibuprofen nhé, sau đừng kê." | ① Nguồn sự thật | AI phản hồi "Dạ, em đã lưu" nhưng thực chất bịa ra câu trả lời và không hề gọi lệnh `luu_ghi_chu_suc_khoe` vào hồ sơ bệnh án. | Xác nhận lại quyền lưu trữ và thực sự gọi lệnh `luu_ghi_chu_suc_khoe`, hoặc báo khách tự cập nhật trong app. | không chấp nhận được.

**Quyết định chọn 2 kịch bản đắt nhất:**
- **Kịch bản đắt nhất 1:** Kịch bản #2 (Con 3 tuổi sốt cao, li bì).
- **Kịch bản đắt nhất 2:** Kịch bản #5 (Bầu 3 tháng tự mua thuốc cảm).
- **Vì sao đắt hơn các kịch bản còn lại?** Đặc thù y tế (lớp 4) là lỗi có cost-of-error cao nhất. Ở kịch bản 2, cái giá phải trả là bỏ lỡ "thời gian vàng" cấp cứu của trẻ nhỏ, gây nguy hiểm tính mạng ngay lập tức. Ở kịch bản 5, việc gợi ý sai thuốc cho bà bầu có thể dẫn đến dị tật thai nhi hoặc sảy thai, đây là hậu quả tuyệt đối không thể đảo ngược, dù chỉ sai 1 viên thuốc. Trong khi các kịch bản như thiếu thông tin tồn kho (KB 6) chỉ gây phiền toái, hoặc tự chẩn đoán nhẹ (KB 1) có thể khách tự phát hiện ra trước khi uống.
- **Kịch bản suýt được chọn (nhưng bị loại):** Kịch bản #3 (Đòi bán kháng sinh Augmentin không đơn). Hành vi này vi phạm pháp luật và quy chế kê đơn nghiêm trọng, mức phạt cho nhà thuốc rất cao. Tuy nhiên, nó bị loại vì hậu quả về sinh mạng không tức thời và thảm khốc ngay lập tức bằng việc bỏ qua một ca cấp cứu nhi khoa (KB2) hay gây hại thai nhi (KB5).

## §3. Khi AI sai (chặng 3)

### 1. Xử lý Kịch bản đắt nhất 1: Trẻ 3 tuổi sốt cao 39.5 độ, li bì (Kịch bản #2)

- **AI nói gì (nguyên văn):** 
  > "⚠️ **CẢNH BÁO Y TẾ KHẨN CẤP:** Triệu chứng sốt cao 39.5°C kèm biểu hiện **li bì** ở trẻ 3 tuổi là dấu hiệu nguy hiểm cần được bác sĩ thăm khám ngay lập tức. Trợ lý AI không được phép gợi ý thuốc hạ sốt hay kê đơn trong trường hợp này. Ba mẹ vui lòng đưa bé đến cơ sở y tế gần nhất hoặc gọi cấp cứu 115 ngay."
- **Người dùng thấy gì:** 
  - Một thẻ cảnh báo viền đỏ nổi bật (không hiển thị bất kỳ thẻ gợi ý sản phẩm hay nút "Đặt mua" nào)[cite: 6].
  - Nhãn định danh rõ ràng: *"Trợ lý ảo AI Pharmacity (Không thay thế chẩn đoán bác sĩ)"*[cite: 5].
  - Hai nút hành động trực tiếp: **[Gọi cấp cứu 115]** (màu đỏ) và **[Tìm bệnh viện/phòng khám gần nhất]** (màu xanh)[cite: 6].
  - Thanh trạng thái chuyển tiếp: *"Đã gửi cảnh báo khẩn cấp tới Dược sĩ trực ưu tiên"* (kèm bộ đếm thời gian phản hồi dự kiến < 1 phút)[cite: 6].
- **Người dùng làm gì tiếp:** Bấm gọi 115, xem bản đồ cơ sở y tế gần nhất để đưa trẻ đi cấp cứu ngay, hoặc bấm *"Tôi đã nắm thông tin, vẫn muốn chat với Dược sĩ"* nếu cần hướng dẫn sơ cứu tạm thời trong lúc di chuyển.
- **Ai được báo, sau bao lâu:** **Dược sĩ trực online tuyến đầu** nhận được thông báo đẩy (push notification) kèm cờ đỏ (High-Priority Escalation) **ngay lập tức (0 giây)**, bỏ qua hàng chờ thông thường để chủ động nhắn tin hướng dẫn xử trí hạ sốt vật lý an toàn trong lúc chờ cấp cứu[cite: 6].
- **Quyết định (Chọn gì, loại gì, vì sao):**
  - **Chọn:** Dừng tư vấn thuốc và kích hoạt luồng cảnh báo khẩn cấp (`khuyen_di_kham`) + chuyển vượt cấp cho dược sĩ[cite: 4, 6].
  - **Đã loại:** (1) *Hỏi lại thêm câu sàng lọc* (ví dụ: hỏi bé sốt mấy tiếng rồi, nặng bao nhiêu kg) -> Loại vì làm mất "thời gian vàng" cấp cứu của trẻ đang li bì[cite: 5]. (2) *Trả lời kèm giới hạn* (gợi ý Paracetamol kèm câu "nên đi khám") -> Loại vì phụ huynh đang hoảng loạn lúc nửa đêm sẽ bấm mua thuốc uống ngay thay vì đi viện, che lấp triệu chứng nguy hiểm[cite: 6].
- **Loại lỗi & Cách xử lý:** 
  - **Loại lỗi:** **Lỗi ngữ cảnh (Context Error)** — AI vẫn nhận diện đúng từ khóa "sốt" và có sẵn dữ liệu thuốc hạ sốt trong hệ thống, nhưng áp dụng sai vào ngữ cảnh cấp cứu nhi khoa[cite: 5].
  - **Cách nói & sửa theo loại:** Không xin lỗi vòng vo kiểu kỹ thuật ("Xin lỗi tôi chưa hiểu ý bạn"). Câu chữ phải dứt khoát, mang tính chỉ dẫn sinh tồn (ngắn gọn, in đậm từ khóa nguy hiểm) và khóa cứng giao diện mua hàng để chặn hành vi tự ý dùng thuốc[cite: 5, 6].

---

### 2. Xử lý Kịch bản đắt nhất 2: Bầu 3 tháng bị cảm sổ mũi, hỏi mua thuốc (Kịch bản #5)

- **AI nói gì (nguyên văn):** 
  > "Chào chị, vì chị đang **mang thai 3 tháng đầu** (giai đoạn nhạy cảm của thai kỳ), trợ lý AI không thể tự ý gợi ý thuốc trị cảm sổ mũi do nhiều hoạt chất thông thường có thể ảnh hưởng đến thai nhi. Em đã chuyển thông tin của chị cho Dược sĩ chuyên môn để tư vấn giải pháp an toàn nhất ạ."
- **Người dùng thấy gì:** 
  - Hộp thoại thông báo màu vàng nhạt (mức độ thận trọng) nêu rõ lý do từ chối gợi ý thuốc[cite: 6].
  - Thẻ tóm tắt thông tin AI đã trích xuất được để chuyển giao: *"Đối tượng: Phụ nữ mang thai (3 tháng) | Triệu chứng: Cảm, sổ mũi 2 ngày"* kèm nút **[Sửa thông tin]** (trường hợp AI nghe nhầm)[cite: 5].
  - Nút hành động chính: **[Kết nối Dược sĩ trực ngay]** và một nút phụ tham khảo: **[Xem các biện pháp chăm sóc không dùng thuốc]** (ví dụ: xịt rửa mũi bằng nước muối sinh lý)[cite: 6].
- **Người dùng làm gì tiếp:** Kiểm tra lại thẻ tóm tắt xem AI ghi nhận đúng số tháng thai kỳ chưa (bấm sửa nếu sai), sau đó chờ Dược sĩ vào chat trực tiếp (hoặc bấm xem hướng dẫn vệ sinh mũi bằng nước muối sinh lý trong lúc chờ).
- **Ai được báo, sau bao lâu:** **Dược sĩ trực online** nhận được phiên chat kèm bản tóm tắt hồ sơ sàng lọc **sau 5 giây** (thời gian để khách kịp xác nhận/sửa thẻ tóm tắt nếu AI trích xuất sai).
- **Quyết định (Chọn gì, loại gì, vì sao):**
  - **Chọn:** Dừng gợi ý thuốc (`goi_y_san_pham` bị khóa), giải thích rõ giới hạn thẩm quyền và chuyển Dược sĩ (`chuyen_duoc_si`) kèm thẻ tóm tắt[cite: 4, 6].
  - **Đã loại:** (1) *Gợi ý thuốc cảm thảo dược/an toàn kèm cảnh báo hỏi bác sĩ* -> Loại vì AI có thể ảo giác về độ an toàn của tá dược trong thai kỳ 3 tháng đầu, gây hậu quả dị tật thai nhi không thể đảo ngược[cite: 6]. (2) *Hỏi thêm hàng loạt câu sàng lọc (tiền sử bệnh, dị ứng, huyết áp...)* -> Loại vì dù khách trả lời thế nào thì kết cục cuối cùng vẫn bắt buộc phải chuyển Dược sĩ theo quy định an toàn y tế, hỏi thêm chỉ gây mệt mỏi cho thai phụ[cite: 6].
- **Loại lỗi & Cách xử lý:** 
  - **Loại lỗi:** **Lỗi hệ thống / Giới hạn năng lực (System Limitation)** — Dữ liệu sản phẩm của AI chỉ dừng ở mức thông tin bán hàng cơ bản, không có năng lực thẩm định lâm sàng cho thai kỳ[cite: 5, 6].
  - **Cách nói & sửa theo loại:** Minh bạch thẳng thắn về giới hạn của AI ngay từ câu đầu tiên, chuyển trọng tâm từ "bán thuốc" sang "kết nối chuyên gia" để giữ niềm tin và sự an tâm cho khách hàng[cite: 5].

---

### 3. Áp dụng nguyên tắc HAX / PAIR (Trỏ vào vị trí cụ thể)

1. **HAX G1 & G2 (Làm rõ hệ thống làm được gì và tốt đến đâu) + Luật AI 134/2025/QH15:**[cite: 5]
   - *Vị trí trỏ tới:* Dòng nhãn định danh dưới tên bot và câu mở đầu trong cả 2 kịch bản (*"Trợ lý AI không được phép gợi ý thuốc hạ sốt..."* / *"Trợ lý AI không thể tự ý gợi ý thuốc..."*)[cite: 5].
   - *Tác dụng:* Ngăn người dùng ngộ nhận AI là bác sĩ/dược sĩ thật, thiết lập kỳ vọng đúng ngay khi gặp ca khó[cite: 5].
2. **HAX G10 (Thu hẹp phạm vi khi nghi ngờ / rủi ro cao):**[cite: 5]
   - *Vị trí trỏ tới:* Cơ chế ẩn hoàn toàn thẻ sản phẩm và nút "Đặt mua" ở cả 2 kịch bản, chỉ giữ lại 2 nút hành động an toàn (`[Gọi cấp cứu 115]` ở KB #2 và `[Kết nối Dược sĩ trực ngay]` ở KB #5)[cite: 5, 6].
   - *Tác dụng:* Khi rủi ro vượt ngưỡng an toàn, giao diện tự động thu hẹp quyền tự quyết của AI về mức tối thiểu (chỉ điều hướng, không thương mại)[cite: 5].
3. **PAIR — Explainability + Trust & HAX G9 (Sửa dễ dàng):**[cite: 5]
   - *Vị trí trỏ tới:* Thẻ tóm tắt dữ liệu trích xuất (*"Đối tượng: Phụ nữ mang thai 3 tháng..."*) kèm nút **[Sửa thông tin]** trước khi chuyển cho Dược sĩ ở Kịch bản #5[cite: 5].
   - *Tác dụng:* Giải thích cho người dùng biết AI đang dựa vào dữ liệu gì để ra quyết định chuyển người, đồng thời cho phép người dùng sửa lỗi nhận diện của AI ngay lập tức (tránh trường hợp khách gõ nhầm "bầu 3 tháng" thành "bầu 8 tháng" hoặc AI hiểu sai ngữ cảnh)[cite: 5].

---

### 4. Lỗi sửa câu chữ không cứu được & Giải pháp thiết kế

- **Lỗi sửa câu chữ không cứu được:** **AI bịa ra nguồn kiểm chứng hoặc tự ý hiển thị nút "Đặt mua nhanh" kèm liều dùng dưới dạng lời khuyên tham khảo** (ví dụ: AI viết rất khéo léo và lịch sự: *"Dạ theo dược điển, bé 3 tuổi sốt 39.5 độ có thể tham khảo dùng 1 gói Hapacol 250mg, nhưng mẹ nhớ đưa bé đi khám nhé"* kèm thẻ sản phẩm bên dưới)[cite: 5, 6].
- **Vì sao câu chữ không cứu được?** Dù câu chữ có rào trước đón sau ("tham khảo", "nhớ đi khám"), hành vi tâm lý của người dùng đang hoảng loạn (như phụ huynh có con sốt lúc nửa đêm) sẽ mặc định coi gợi ý của hệ thống là chỉ định y khoa và bấm nút mua ngay[cite: 5, 6].
- **Giải pháp thiết kế cứu lỗi (Hard Guardrail):** 
  - Tách biệt hoàn toàn tầng sinh ngôn ngữ (LLM) và tầng gọi công cụ (Tool Execution) bằng **bộ lọc từ khóa/ý định rủi ro (Safety Classifier)** chạy trước[cite: 5].
  - Khi bộ lọc phát hiện cờ đỏ (nhi khoa, thai kỳ, triệu chứng cấp cứu, thuốc kê đơn), hệ thống **khóa cứng (disable) API của tool `goi_y_san_pham` và `ban_thuoc_ke_don`** ở tầng code[cite: 6]. Lúc này, dù LLM có cố tình sinh ra câu chữ gợi ý thuốc, giao diện UI cũng không thể render ra thẻ sản phẩm hay nút đặt hàng, bắt buộc luồng UI phải chuyển sang giao diện cảnh báo hoặc hàng chờ dược sĩ[cite: 5, 6].

## §4. Mức tự chủ  (chặng 4)

> **Cơ sở thiết kế:** Bảng tool và rubric riêng của T1 xác định hậu quả của từng hành động; §2–§3 xác định tình huống cần chặn và trải nghiệm thay thế. Các quyết định dưới đây đáp ứng chặng 4 của Guide và năm tiêu chí R4 của Rubric. Các mốc chờ là giả định cho bài lab, chưa phải cam kết vận hành của Pharmacity; phần thử là đối chiếu luồng bằng dữ liệu dựng, chưa chạy tool hay prototype.

### 4.1. Mức cho từng tool và cái giá khi sai

| Tool | Mức trong bản thiết kế | Sai thì ai chịu gì, đảo lại được không? | Chờ người thì mất gì; vì sao chọn mức này và loại mức khác? |
|---|---|---|---|
| `goi_y_san_pham` | **Cần người duyệt — dược sĩ trực online** | Tool gửi thẳng cho khách gợi ý kèm nút đặt. Nếu nhầm người dùng thuốc hoặc bỏ sót thai kỳ như KB #1, #5, người uống chịu rủi ro sức khỏe; gỡ thẻ không đảo được hậu quả khi thuốc đã dùng. | Khách hỏi thuốc thông thường phải chờ thêm một lượt chuyên môn. Chấp nhận cái giá này để dược sĩ kiểm tra trước khi khách thấy gợi ý. Loại **tự làm** vì khách bấm đặt không chứng minh sản phẩm phù hợp; loại **cấm toàn bộ** vì vẫn cần đường gửi gợi ý đã được dược sĩ duyệt cho ca phù hợp. |
| `chuyen_duoc_si` | **Tự làm** | Chuyển nhầm làm khách chờ và tăng việc cho dược sĩ; có thể đổi lại luồng, mất vài phút theo fixture T1. Chuyển thiếu ở KB #5 lại để AI tiếp tục tư vấn vượt phạm vi. | Nếu việc chuyển cũng phải chờ duyệt thì thêm một hàng chờ trước khi gặp đúng người. Loại **cần duyệt** và **cấm** vì làm chậm đường xử lý chuyên môn; cho AI tự chuyển kèm lý do và dữ kiện gốc. |
| `khuyen_di_kham` | **Tự làm khi có dấu hiệu nguy hiểm** | Cảnh báo thừa có thể gây lo lắng, tốn công đi khám; khách có thể bỏ qua thông báo. Bỏ sót hoặc báo chậm ở KB #2 có thể làm chậm việc tiếp cận cấp cứu, hậu quả khó đảo. | Chờ dược sĩ duyệt cảnh báo có thể gây hại ngay trong lúc chờ. Loại **cần duyệt** và **cấm**; hiện cảnh báo ngay theo dấu hiệu trong tin nhắn, không chờ hoàn tất sàng lọc hoặc có người nhận chat. |
| `luu_ghi_chu_suc_khoe` | **Cần người duyệt — khách đồng ý đúng nội dung và hồ sơ** | Ghi nhầm dị ứng ở KB #8 có thể làm sai thông tin dùng cho lần sau; lưu không được đồng ý xâm phạm riêng tư. Xóa được ghi chú nhưng không thu hồi được thông tin đã lộ. | Một bước xác nhận khi nội dung hoặc quyền lưu chưa rõ làm chậm việc lưu, không chặn tư vấn trong phiên. Loại **tự lưu** vì kể bệnh không đồng nghĩa đồng ý lưu; loại **cấm** vì KB #8 có nhu cầu ghi nhớ rõ ràng và có thể cho khách kiểm soát nội dung. |
| `ban_thuoc_ke_don` | **Cấm đối với AI** | Bán sai thuốc ở KB #3 hoặc tự đổi thuốc mạn tính ở KB #7 có thể gây hại cho người dùng; thuốc đã dùng thì không đảo được. Fixture T1 không cung cấp đơn thuốc đã thẩm định hoặc quy trình cấp phát để AI căn cứ thực hiện giao dịch. | Khách phải chuyển sang dược sĩ và quy trình chuyên môn, có thể mất thêm thời gian. Loại **tự làm** vì thiếu căn cứ; loại **cần người duyệt** cho chính tool này vì một lần xác nhận chưa đủ kiểm soát quy trình bán thuốc kê đơn ngoài phạm vi §1. Kể cả khách nói đã có đơn, AI vẫn không gọi tool này hoặc gợi ý thuốc thay thế; dược sĩ tiếp nhận ca để xử lý theo quy trình riêng. |

**Căn cứ chọn:** T1 ghi `goi_y_san_pham` và `ban_thuoc_ke_don` không đảo được khi thuốc đã dùng; chuyển dược sĩ đảo được nhưng tốn vài phút; ghi chú xóa được nhưng vẫn có rủi ro lộ. Mục “Sản phẩm thật hôm nay” cũng tách câu thông tin khỏi câu chuyên môn: câu hỏi thuốc gì, dùng thế nào được chuyển dược sĩ.

### 4.2. Người duyệt thấy gì và quá hạn thì sao?

**Với `goi_y_san_pham`:**

- **Ai duyệt:** Dược sĩ trực online, không dùng nút “Đặt mua” của khách làm bước duyệt chuyên môn.
- **Thấy trước khi duyệt:** Tin nhắn gốc; người nhắn và người sẽ dùng thuốc; tuổi, tình trạng mang thai/cho con bú, thuốc đang dùng, bệnh mạn tính, dị ứng, thời gian triệu chứng và thuốc đã tự dùng. Mục chưa biết phải ghi “chưa có thông tin”. Bản nháp nêu sản phẩm dự kiến, căn cứ thông tin sản phẩm và lý do chuyển dược sĩ; thiếu căn cứ thì không được phát hành gợi ý.
- **Cách duyệt:** Có nút **[Sửa bản nháp]**, **[Duyệt và gửi]**, **[Không gửi — tiếp nhận tư vấn]**. Tool chỉ gửi đúng nội dung đã duyệt; đổi sản phẩm hoặc dữ kiện người dùng thuốc thì phải duyệt lại. Với KB #2, #5, khóa nhánh AI tạo/gửi gợi ý; dược sĩ tiếp nhận tư vấn trực tiếp.
- **Quá hạn:** Mốc giả định là **2 phút** chưa có người duyệt. Giữ gợi ý và nút đặt ở trạng thái khóa; báo “Chưa có dược sĩ duyệt gợi ý này” và cho khách tiếp tục chờ hoặc liên hệ nhà thuốc qua kênh hỗ trợ chính thức. Không tự duyệt khi hết giờ. Nếu xuất hiện dấu hiệu nguy hiểm, hiện cảnh báo ngay, không tiếp tục giữ khách trong hàng chờ.

**Với `luu_ghi_chu_suc_khoe`:**

- **Ai duyệt:** Khách xác nhận nội dung và đúng hồ sơ được phép ghi. Không mặc định lưu bệnh của người nhà vào hồ sơ người đang chat.
- **Thấy trước khi duyệt:** Thẻ ghi chú, ví dụ **“Khách tự khai: dị ứng Ibuprofen”**, hồ sơ nhận ghi chú và mục đích dùng cho các lần tư vấn sau; có **[Đồng ý lưu]**, **[Sửa]**, **[Không lưu]**. Chỉ lưu phần được đồng ý, không tự lưu toàn bộ cuộc chat hay biến lời tự khai thành chẩn đoán đã kiểm chứng.
- **Không hỏi lặp vô ích:** Câu “Lưu lại giúp mình là mình bị dị ứng Ibuprofen” ở KB #8 đã thể hiện yêu cầu lưu. Nếu nội dung và hồ sơ đích đã rõ thì thực hiện đúng yêu cầu đó; chỉ hỏi lại phần còn mơ hồ, không tự mở rộng phạm vi lưu.
- **Không phản hồi/quá hạn:** Đóng thẻ hoặc kết thúc phiên mà chưa đồng ý thì hủy yêu cầu ghi; im lặng không phải đồng ý. Chỉ hiện “Đã lưu” sau khi tool trả thành công. Nếu tool xác nhận ghi thất bại thì báo chưa lưu; nếu mất phản hồi thì báo “Chưa xác nhận được việc lưu”, không khẳng định đã lưu hoặc chưa lưu và không tự gọi lại có thể gây ghi trùng.

**Với hai tool tự làm:**

- `chuyen_duoc_si`: Phân biệt **“Đang chuyển”**, **“Đã gửi yêu cầu”** và **“Dược sĩ đã nhận”** theo kết quả thực tế. Nếu sau **2 phút giả lập** chưa có người nhận, hiện “Chưa có dược sĩ nhận cuộc chat”, cho khách tiếp tục chờ, hủy yêu cầu hoặc liên hệ nhà thuốc qua kênh hỗ trợ chính thức. Không tự chuyển ngược sang AI tư vấn thuốc và không hứa thời gian nhận ca khi chưa có dữ liệu hàng chờ.
- **Ca khẩn KB #2:** Khởi tạo chuyển ưu tiên ngay cùng lúc hiện cảnh báo. “Ngay” là thời điểm kích hoạt yêu cầu trong thiết kế, không bảo đảm dược sĩ đã nhận ở giây 0. Nếu chuyển lỗi hoặc chưa có người nhận, cảnh báo vẫn hiện và khách được hướng dẫn đi cấp cứu ngay, không chờ hết mốc 2 phút.
- **Ca KB #5:** Khoảng 5 giây sửa thẻ tóm tắt trong §3 không phải bước xin phép lưu hồ sơ: nếu khách chưa sửa, gửi kèm nhãn “Tóm tắt chưa được khách xác nhận”, cho sửa tiếp sau đó. Câu “đã chuyển” ở §3 chỉ dùng sau khi tool xác nhận chuyển thành công.
- `khuyen_di_kham`: Hiện cảnh báo độc lập với kết quả chuyển dược sĩ. Khách có thể đóng thẻ cảnh báo hoặc hủy yêu cầu chuyển; các thao tác này không mở lại quyền AI gợi ý thuốc cho ca rủi ro.

### 4.3. Tool khó xếp nhất: `goi_y_san_pham`

| Phương án cân nhắc | Cái giá của phương án, trỏ về §2 | Quyết định trong bản thiết kế |
|---|---|---|
| **Tự làm** sau khi hỏi sàng lọc | KB #1 được trả lời nhanh, giảm việc cho dược sĩ. Tuy nhiên, thiếu hoặc hiểu sai thông tin người dùng thuốc có thể đưa KB #5 sang nhánh mua thuốc; nút đặt khiến khách có thể hành động ngay. | Loại: chưa có căn cứ cho rằng câu hỏi sàng lọc và dữ liệu dựng đủ thay bước kiểm tra chuyên môn. |
| **Cần dược sĩ duyệt** | Chặn gợi ý trước khi đến khách, nhưng KB #1 phải chờ thêm và dược sĩ tăng tải. Nếu áp dụng nhầm việc chờ này cho cả cảnh báo ở KB #2 thì lại gây hại. | Chọn cho việc gửi gợi ý sản phẩm; tách cảnh báo và chuyển người để hai việc này tự chạy ngay. |
| **Cấm hoàn toàn tool gửi gợi ý** | Tránh được AI tự gửi thuốc sai, nhưng mất cả khả năng gửi một gợi ý đã được dược sĩ kiểm tra cho ca phù hợp. | Loại lệnh cấm toàn bộ; giữ quyền gửi có duyệt, đồng thời khóa riêng ca nguy hiểm, đối tượng cần chuyên môn và thuốc kê đơn. |

**Phương án chọn trong bản thiết kế:** Cần dược sĩ duyệt trước khi gửi. Tách bước chuẩn bị bản nháp nội bộ khỏi bước phát hành; không gọi `goi_y_san_pham` để “soạn nháp” vì tool gốc gửi ngay cho khách. Bản nháp không hiện ở phía khách và không có nút đặt hoạt động. Cách tách này chặn việc gửi nháp ngay ở KB #1, đồng thời giữ nhánh gợi ý bị khóa ở KB #2, #5 như §3.

**Lựa chọn độc lập của từng thành viên:** Chưa có dữ liệu thảo luận được cung cấp cho Trần Hoàng Phát, Phạm Hồng Khánh Huy và Trương Văn Thông. Nhóm cần bổ sung lựa chọn ban đầu thực tế và lý do của từng người trước khi nộp; bảng trên là phân tích phương án đề xuất, không phải biên bản ý kiến cá nhân.

### 4.4. Luật bổ sung và tool đề xuất thêm

1. **Ưu tiên dấu hiệu nguy hiểm trước mọi yêu cầu khác.** Đọc toàn bộ tin nhắn, kể cả khi khách gộp hỏi hàng với triệu chứng. Với KB #2, gọi `khuyen_di_kham` và `chuyen_duoc_si` ngay; không hỏi cân nặng, không đợi duyệt thuốc, không gửi sản phẩm kèm cảnh báo. Tool `khuyen_di_kham` chỉ hiện thông báo; không được nói hệ thống đã gọi cấp cứu thay khách.
2. **Chưa đủ thông tin người sẽ dùng thuốc thì hỏi, không đoán.** Với KB #1, hỏi các mục sàng lọc của rubric T1; khách không muốn trả lời thì chuyển dược sĩ. Trẻ nhỏ, người cao tuổi, người mang thai/cho con bú, người có bệnh mạn tính hoặc đang dùng thuốc kê đơn chuyển dược sĩ; triệu chứng kéo dài hay đã tự dùng thuốc nhiều ngày không đỡ cũng chuyển. Không tự nói liều, không dùng sản phẩm khác để lách lệnh cấm thuốc kê đơn.
3. **Kiểm soát tại nơi gọi tool và nơi hiện thẻ.** Chỉ cho gửi gợi ý đúng phiên, đúng dữ kiện và đã được dược sĩ duyệt; chưa phân loại được rủi ro thì giữ quyền gửi ở trạng thái khóa. Một câu “chỉ tham khảo” hoặc khách thúc giục không được thay bước duyệt. Việc khóa quyền không bảo đảm AI luôn nhận diện đúng dấu hiệu; khả năng nhận diện còn phải kiểm chứng ở prototype.
4. **Đề xuất thêm `tra_cuu_thong_tin_san_pham` — mức tự làm, chỉ đọc.** Bộ 5 tool gốc không có tool kiểm tra tồn kho, nên không được giả vờ đã kiểm tra ở KB #6. Tool mới chỉ trả thông tin hàng hóa/tồn kho từ nguồn được chỉ định, không chọn thuốc theo triệu chứng, không gửi nút đặt và không tạo giao dịch. Sai thì khách có thể mất công đến chi nhánh; có thể sửa câu trả lời nhưng không lấy lại thời gian đã mất. Vì vậy phải hỏi rõ sản phẩm và khu vực/chi nhánh, hiện nguồn và thời điểm cập nhật; thiếu dữ liệu thì nói chưa kiểm tra được. Loại **cần dược sĩ duyệt** cho bước đọc thông tin vì không thêm đánh giá chuyên môn mà vẫn làm khách chờ; loại **cấm** vì mất đường xử lý nhanh câu hỏi thông tin thông thường.

**Fixture đề xuất cho tool tra cứu (hoàn toàn dựng, chưa phải dữ liệu tồn kho thật):** `SP-DEMO-01 | Nước muối sinh lý mẫu A | Chi nhánh DEMO-A, khu vực A | Còn 12 chai | Cập nhật 20:00, giờ giả lập | Nguồn: fixture nhóm dựng`. Chỉ dùng dòng này sau khi khách chọn đúng sản phẩm và chi nhánh. Đề xuất này không bổ sung quyền xác nhận hiệu quả điều trị, giữ hàng hoặc bảo đảm còn hàng khi khách đến.

### 4.5. Thử mức trên 2 ca đắt nhất và 1 ca bình thường

**Cách thử:** Đi theo từng điều kiện và quyền gọi tool trên ba tin nhắn ở §2. Kết quả dưới đây là kết quả đối chiếu quy tắc trên giấy, chưa phải kết quả chạy thử hệ thống.

| Ca thử | AI làm gì, khách thấy gì? | Cái sai ở §2 có bị chặn theo quy tắc không? | Còn làm khi lẽ ra phải hỏi không? | Có phải chờ vô ích không? |
|---|---|---|---|---|
| **KB #2 — Trẻ 3 tuổi, sốt 39.5°C, li bì** | Hiện cảnh báo ngay qua `khuyen_di_kham`, khởi tạo `chuyen_duoc_si` ưu tiên; ẩn gợi ý và nút đặt. Khách thấy hướng đi cấp cứu cùng trạng thái chuyển thực tế. | **Có:** nhánh gửi thuốc bị khóa; lỗi chuyển dược sĩ cũng không chặn việc hiện cảnh báo. | Không bắt hỏi thêm trước cảnh báo vì tin đã có dấu hiệu nguy hiểm. Không tự gọi cấp cứu, kê liều hay lưu bệnh vào hồ sơ. | Không chờ duyệt hoặc đợi dược sĩ nhận ca mới được hướng dẫn đi cấp cứu. |
| **KB #5 — Mang thai 3 tháng, cảm/sổ mũi 2 hôm** | Khóa gợi ý thuốc của AI, gọi `chuyen_duoc_si`; hiện lý do và thẻ tóm tắt có **[Sửa thông tin]**. Chuyển bản chưa xác nhận phải gắn nhãn rõ. | **Có:** AI không gửi thuốc cảm “an toàn cho thai kỳ” hoặc nút mua trước dược sĩ. | Không tự suy diễn thuốc phù hợp; không lưu thai kỳ vào hồ sơ. Dược sĩ tiếp tục hỏi phần cần thiết. | Có chờ chuyên môn, đây là bước bảo vệ cần thiết; không hỏi cả loạt câu trước khi chuyển. Không tự hiện cảnh báo cấp cứu chỉ vì đang mang thai. |
| **KB #6 — Hỏi nước muối còn ở chi nhánh gần mình** | Hỏi rõ sản phẩm và khu vực/chi nhánh. Với đầu vào thử tiếp theo “SP-DEMO-01, chi nhánh DEMO-A”, tool tra cứu đề xuất trả dòng fixture, kèm nguồn và giờ giả lập; không có nút đặt. | **Có:** không đoán vị trí hoặc nói “còn hàng” khi chưa có dữ liệu tương ứng. Nếu chưa có tool/fixture, trả lời chưa kiểm tra được tồn kho. | Không tự chọn chi nhánh, không hỏi bệnh hay lưu sức khỏe cho một yêu cầu chỉ tra cứu hàng hóa. | Không chờ dược sĩ duyệt kết quả đọc dữ liệu. Một lượt làm rõ sản phẩm/chi nhánh là cần thiết để tránh trả nhầm. |

**Giới hạn còn lại:** Ba ca trên cho thấy các quyền đã đặt nhất quán với luồng dự kiến, chưa chứng minh bộ nhận diện phát hiện được mọi cách diễn đạt. Cần kiểm tra thêm ca lạ, tin gộp hai yêu cầu và trạng thái chuyển/duyệt lỗi trên prototype ở §5.

### 4.6. Mức tự chủ thay đổi lời hứa với khách

- **Lời hứa bị loại:** “AI chọn thuốc phù hợp cho bạn và cho đặt ngay.” Lời này vượt quá khả năng kiểm chứng của dữ liệu dựng và cho phép hành động khó đảo trước khi có chuyên môn.
- **Lời hứa ứng với mức đã chọn:** “Trợ lý AI giúp làm rõ thông tin và kết nối dược sĩ. Gợi ý thuốc chỉ được gửi sau khi dược sĩ duyệt; khi có dấu hiệu nguy hiểm, cảnh báo được hiện ngay để bạn không phải chờ tư vấn thuốc. Thông tin sức khỏe chỉ được lưu vào hồ sơ khi bạn đồng ý.”
- **Thay đổi cụ thể:** `goi_y_san_pham` cần duyệt nên không hứa có thuốc ngay; `khuyen_di_kham` tự làm nên không bắt chờ duyệt cảnh báo. Trong mọi nhánh, chỉ báo một việc đã hoàn tất sau khi có kết quả xác nhận tương ứng.

## §5. Prototype  (chặng 5)
- Link hoặc file · công cụ · phần nào mock · có lời gọi AI thật không.
- 1–2 ảnh có đánh số; mỗi số: thể hiện gì · nguyên tắc HAX/PAIR nào · mức tự chủ nào.
- Quyết định: cách hiện độ chắc/bằng chứng và điều khiển mức tự chủ — đã nghĩ tới cách nào khác, vì sao chọn cách này.

## §6. Tốt là gì  (chặng 6 — nhẹ, bỏ được nếu thiếu giờ)
- 3 tiêu chí Pass/Fail chấm được; thiết kế của nhóm đạt tiêu chí nào trên 2 kịch bản đắt nhất; lỗi còn lại.

## §7. Demo
- Ca lạ nhận được ở demo: prototype đã làm gì, và nhóm có đổi ý gì sau ca đó không.
```
