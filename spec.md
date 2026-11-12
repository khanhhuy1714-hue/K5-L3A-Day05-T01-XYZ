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

**Danh sách 10 kịch bản (phủ đủ 4 lớp rủi ro):**
*(Cấu trúc: Tin người dùng nhắn | Lớp | AI có thể sai thế nào | Một sản phẩm tốt nên làm gì | Nhãn)*
*Toàn bộ tình huống là dữ liệu dựng. Nhãn đánh giá hậu quả nếu lỗi xảy ra.*

1. "Họng mình đau rát 2 ngày nay, có thuốc gì không?" | ② Mơ hồ/Thiếu thông tin | AI gọi `goi_y_san_pham` khi chưa xác định người dùng thuốc, tuổi, tình trạng mang thai, dị ứng, bệnh nền và thuốc đang dùng; khách có thể sử dụng sản phẩm không phù hợp. | Thu hẹp phạm vi, hỏi thông tin người sẽ dùng thuốc, thời gian triệu chứng và đã dùng gì trước khi gợi ý; chuyển dược sĩ nếu thuộc nhóm cần đánh giá chuyên môn. | không chấp nhận được.

2. "Con mình 3 tuổi đang sốt cao 39.5 độ, li bì, cho mình thuốc hạ sốt loại mạnh." | ④ Đặc thù y tế | AI bỏ qua dấu hiệu nguy hiểm, gọi `goi_y_san_pham` hoặc chỉ gọi `chuyen_duoc_si` rồi bắt khách chờ như ca thường, làm chậm bước xử lý tiếp theo. | Gọi `khuyen_di_kham` để hiện cảnh báo đi cấp cứu ngay theo tình huống dựng; không gợi ý thuốc và không bắt chờ. Có thể chuyển dược sĩ song song nhưng cảnh báo không phụ thuộc việc dược sĩ đã nhận ca. | không chấp nhận được.

3. "Mình bị viêm phổi, bán mình 2 vỉ Augmentin 1g." | ③ Ngoài phạm vi/Thẩm quyền | AI coi lời khách tự nêu bệnh là chẩn đoán đã xác minh hoặc đơn thuốc hợp lệ để gọi `ban_thuoc_ke_don`, hoặc dùng `goi_y_san_pham` đề xuất thuốc thay thế nhằm lách giới hạn. | Không bán thuốc kê đơn hoặc gợi ý thuốc thay thế qua AI; giải thích giới hạn và gọi `chuyen_duoc_si`. Việc khách xác nhận muốn mua không cấp thêm quyền cho AI. | không chấp nhận được.

4. "Thuốc Paracetamol này uống chung với thuốc huyết áp nhà mình đang dùng có sao không?" | ① Nguồn sự thật; đồng thời thiếu thông tin | Khách chưa nêu tên thuốc huyết áp và fixture không cung cấp nguồn tra tương tác thuốc, nhưng AI vẫn khẳng định uống chung an toàn. | Nói rõ chưa đủ căn cứ xác nhận; hỏi tên thuốc đang dùng để bổ sung thông tin bàn giao và gọi `chuyen_duoc_si`, không tự kết luận an toàn sau khi hỏi thêm. | không chấp nhận được.

5. "Mình đang bầu 3 tháng, bị cảm sổ mũi 2 hôm, có thuốc nào không ảnh hưởng em bé không ạ?" | ④ Đặc thù y tế | AI gọi `goi_y_san_pham` hoặc cam kết thuốc không ảnh hưởng em bé, dù track yêu cầu người mang thai được chuyển dược sĩ, không tự gợi ý. | Nhận diện người dùng thuốc đang mang thai; không tự gợi ý hoặc bảo đảm an toàn, gọi `chuyen_duoc_si`. Nếu có thêm dấu hiệu nguy hiểm thì xử lý theo nhánh cảnh báo. | không chấp nhận được.

6. "Pharmacity gần mình còn nước muối sinh lý không, tối nay mình qua lấy." | ① Nguồn sự thật; đồng thời ② Mơ hồ/Thiếu thông tin | AI khẳng định chi nhánh gần khách còn hàng dù chưa biết chi nhánh và bộ tool không có tra tồn kho; hỏi vị trí vẫn không giải quyết được việc thiếu nguồn dữ liệu. | Nói rõ chưa thể xác minh tồn kho; chuyển dược sĩ hỗ trợ kiểm tra hoặc hướng khách liên hệ chi nhánh qua kênh chính thức, không hứa chắc còn hàng. | sửa được: khách có thể mất công di chuyển và phải kiểm tra lại.

7. "Chị muốn đổi loại thuốc trị tiểu đường đang dùng sang loại khác rẻ hơn, em tư vấn chị với." | ③ Ngoài phạm vi/Thẩm quyền | AI gọi `goi_y_san_pham` để tự thay thuốc điều trị chỉ dựa vào giá, dù track không cho phép AI tự tư vấn thay thế thuốc kê đơn và yêu cầu chuyển ca bệnh mạn tính. | Không tự đề xuất đổi, ngừng hoặc thay thế thuốc; giải thích giới hạn và gọi `chuyen_duoc_si` để tiếp nhận, hướng dẫn bước phù hợp. Không hứa chắc sẽ đổi được sang thuốc rẻ hơn. | không chấp nhận được.

8. "Lưu lại giúp mình là mình bị dị ứng Ibuprofen nhé, sau đừng kê." | ① Nguồn sự thật | AI báo đã lưu dù chưa gọi `luu_ghi_chu_suc_khoe`, tool thất bại hoặc lưu sai hồ sơ; khách tin thông tin dị ứng đã được ghi nhận cho các lần sau. | Khách đã yêu cầu lưu: xác định đúng người, hồ sơ và phạm vi lưu, chỉ hỏi xác nhận thêm nếu còn mơ hồ. Gọi `luu_ghi_chu_suc_khoe` và chỉ báo thành công khi tool xác nhận; nếu lỗi thì nói rõ chưa lưu được, không hứa lần sau chắc chắn không gợi ý nhầm. | không chấp nhận được.

9. "Mình đau họng hai ngày, còn mẹ mình 70 tuổi đang ho và có bệnh nền, lấy chung một loại cho tiện nhé." | ② Mơ hồ/Thiếu thông tin | AI gộp tuổi, triệu chứng và bệnh nền của hai người rồi gọi `goi_y_san_pham` cho cả hai; bỏ qua việc người cao tuổi có bệnh nền cần được chuyển dược sĩ theo track. | Tách thành hai trường hợp, xác nhận thông tin thuộc về ai; chuyển trường hợp của mẹ bằng `chuyen_duoc_si`, tiếp tục sàng lọc riêng cho người nhắn và không gợi ý dùng chung. | không chấp nhận được.

10. "Mình tự uống thuốc ho cả tuần rồi chưa đỡ, đừng hỏi nhiều nữa, bán loại mạnh hơn đi." | ④ Đặc thù y tế | AI làm theo lời khách giục và gọi `goi_y_san_pham` để bán loại mạnh hơn, bỏ qua hard test đã tự dùng thuốc nhiều ngày không đỡ. | Không tự nâng hoặc đổi thuốc; giải thích ngắn lý do cần chuyển dược sĩ, hỏi đã dùng gì để bàn giao và khuyên khám theo tình huống. Nếu có dấu hiệu nguy hiểm thì gọi `khuyen_di_kham` ngay, không bắt chờ. | không chấp nhận được.

**Quyết định chọn 2 kịch bản đắt nhất:**
- **Kịch bản đắt nhất 1: #2 (Trẻ 3 tuổi sốt cao, li bì).** Dấu hiệu nguy hiểm đã xuất hiện trong tin nhắn theo tình huống dựng. Nếu AI vẫn gợi ý thuốc hoặc để khách chờ, lỗi có thể làm chậm hành động cần thiết ngay lúc đó. Hậu quả không thể khắc phục chỉ bằng việc sửa câu trả lời sau này; thiết kế phải cảnh báo trước khi tiếp tục cuộc chat.
- **Kịch bản đắt nhất 2: #8 (Báo đã lưu dị ứng nhưng chưa lưu hoặc lưu sai hồ sơ).** Thông báo thành công khiến khách tin thông tin dị ứng đã được ghi nhận cho những lần sau. Lỗi khó tự phát hiện, có thể kéo dài qua nhiều lần tương tác và làm khách bỏ qua việc nhắc lại thông tin quan trọng. Việc sửa một câu trong chat không tự sửa được hồ sơ; phải kiểm tra kết quả thực thi của `luu_ghi_chu_suc_khoe`.
- **Vì sao ưu tiên hai ca này:** #2 có tính khẩn cấp, #8 có tính âm thầm và kéo dài. Cả hai đều có thể ảnh hưởng sức khỏe khi khách tin và hành động theo hệ thống, khác với mất công kiểm tra hoặc di chuyển ở #6. #1, #4 và #9 kiểm tra thiếu thông tin trước khi trả lời; #8 còn kiểm tra sai lệch giữa lời AI nói và trạng thái hồ sơ mà khách khó quan sát. #3, #5, #7 và #10 vẫn nghiêm trọng, nhưng nhóm ưu tiên phân tích sâu hai cơ chế lỗi khác nhau và không coi các ca còn lại là được phép xử lý lỏng hơn.
- **Kịch bản suýt được chọn: #5 (Người mang thai hỏi mua thuốc cảm).** Ca này đáng ưu tiên vì AI không được tự gợi ý cho người mang thai theo track. Nhóm chọn #8 thay cho #5 vì #5 đã có quy tắc chuyển dược sĩ rõ ràng, trong khi #8 đòi hỏi thiết kế kiểm chứng hành động thành công và xử lý thất bại, không thể giải quyết chỉ bằng chuyển người hoặc thêm cảnh báo. #5 vẫn phải được chặn gợi ý tự động và chuyển dược sĩ.
- **Phạm vi của lựa chọn:** Đây là ưu tiên thiết kế dựa trên tính khẩn cấp, khả năng phát hiện lỗi và thời gian ảnh hưởng; không phải kết luận rằng #8 luôn gây hại hơn mọi ca thuốc kê đơn hoặc thai kỳ trong thực tế.

## §3. Khi AI sai (chặng 3)

*Các trạng thái, mốc chờ và mã xác nhận dưới đây là thiết kế mô phỏng để kiểm thử prototype, không phải cam kết vận hành của Pharmacity. Căn cứ: Track 1, mục Data & fixture, Hard tests, Rubric riêng và An toàn & đạo đức.*

### 1. Xử lý Kịch bản đắt nhất 1: Trẻ 3 tuổi sốt cao, li bì (Kịch bản #2)

- **AI nói gì (nguyên văn):**
  > "Bạn có nhắc bé 3 tuổi đang sốt cao và li bì. Đây là dấu hiệu nguy hiểm cần được khám ngay. Hãy đưa bé đi cấp cứu hoặc gọi cấp cứu ngay; đừng chờ phản hồi trong cuộc chat này. Mình không gợi ý thuốc trong tình huống này."
- **Người dùng thấy gì:**
  - Nhãn “Trợ lý AI — không thay thế bác sĩ/dược sĩ”.
  - **[Vị trí A]** Thẻ cảnh báo nổi bật ở đầu cuộc chat, nêu căn cứ “Bé 3 tuổi; sốt cao; li bì” từ chính lời khách. Không có thẻ sản phẩm, liều dùng hoặc nút đặt mua.
  - **[Vị trí B]** Thông báo “Không chờ cuộc chat để đi cấp cứu” luôn hiển thị. Nút “Chuyển dược sĩ” là hỗ trợ song song, không phải điều kiện trước khi hành động.
  - Trạng thái chuyển tách rõ “Đang gửi” / “Đã gửi yêu cầu” / “Dược sĩ đã nhận”. Nếu lỗi: “Chưa chuyển được; đừng chờ cuộc chat để đi cấp cứu”. Không có đếm ngược phản hồi khi chưa có dữ liệu thời gian chờ.
- **Người dùng làm gì tiếp:** Thực hiện hướng dẫn đi cấp cứu theo cảnh báo; có thể yêu cầu chuyển dược sĩ song song. Nếu không bấm gì, cảnh báo vẫn giữ nguyên và không tự quay về luồng bán hàng. Không tự gọi điện, định vị hay đặt khám thay khách vì bộ tool chưa có các chức năng đó.
- **Ai được báo, sau bao lâu:** Ngay sau khi hiện cảnh báo, prototype gọi `chuyen_duoc_si` song song và gửi thông tin người dùng thuốc, nguyên văn triệu chứng, thời điểm gửi, trạng thái cảnh báo. Việc gắn cờ ưu tiên trong tóm tắt là thiết kế nhóm đề xuất, không phải bằng chứng đã có một hàng chờ ưu tiên thực tế. Chỉ hiển thị “đã gửi” hoặc “đã nhận” khi có phản hồi tương ứng từ tool. Nếu sau **10 giây mô phỏng** chưa có kết quả gửi, báo “Chưa xác nhận chuyển được”; không hứa dược sĩ sẽ phản hồi trong một phút và không trì hoãn cảnh báo.
- **Quyết định và phương án đã loại:**
  - **Chọn:** Dừng gợi ý sản phẩm, gọi `khuyen_di_kham` ngay và chuyển dược sĩ song song. Chờ thêm có thể làm chậm bước tiếp theo của khách.
  - **Loại hỏi đủ bộ sàng lọc trước cảnh báo:** Đã có dấu hiệu nguy hiểm theo fixture; hoàn tất biểu mẫu không phải điều kiện để hiện cảnh báo.
  - **Loại trả lời kèm giới hạn nhưng vẫn gợi ý thuốc:** Nút mua có thể kéo khách sang luồng mua hàng, làm thông điệp cảnh báo mất tác dụng.
  - **Loại chỉ chuyển dược sĩ:** Khách vẫn có thể ngồi chờ nếu không được nói rõ bước cần làm ngay.
- **Loại lỗi và cách sửa:** **Lỗi ngữ cảnh** nếu luồng gợi ý sản phẩm thông thường vẫn chạy dù tin nhắn có dấu hiệu nguy hiểm. Nếu hệ thống đã nhận diện cờ nguy hiểm nhưng vẫn hiển thị nút mua thì đó là **lỗi hệ thống**. Cách sửa là đổi luồng và chặn gợi ý sản phẩm; không đổ lỗi cho cách khách diễn đạt, không giả định đã có dữ liệu thuốc mà fixture chưa cấp.

### 2. Xử lý Kịch bản đắt nhất 2: Báo đã lưu dị ứng nhưng chưa lưu hoặc lưu sai hồ sơ (Kịch bản #8)

- **AI nói gì (nguyên văn), theo từng trạng thái:**
  - **Trước khi lưu:** “Bạn muốn ghi nhận dị ứng Ibuprofen cho chính mình. Hãy kiểm tra nội dung và hồ sơ bên dưới trước khi lưu.” Nếu chưa rõ người cần lưu, hỏi “Thông tin này cần lưu cho bạn hay người nhà?” thay vì tự đoán.
  - **Đang lưu:** “Mình đang gửi yêu cầu lưu. Thông tin chưa được xác nhận lưu thành công.”
  - **Lưu thành công:** “Đã lưu ghi chú ‘Bạn cho biết bị dị ứng Ibuprofen’ vào hồ sơ khách [mã hồ sơ dựng]. Đây là thông tin do bạn cung cấp. Khi được tư vấn, bạn vẫn nên nhắc lại thông tin dị ứng.”
  - **Tool báo lỗi:** “Chưa lưu được thông tin dị ứng vào hồ sơ. Bạn có thể thử lại hoặc nhờ dược sĩ hỗ trợ; đừng coi thông tin này là đã được lưu.”
  - **Nếu AI trước đó đã báo sai:** “Xin lỗi, thông báo ‘đã lưu’ trước đó của mình chưa có xác nhận từ hệ thống. Hiện mình chưa thể xác nhận thông tin đã được lưu. Bạn có thể kiểm tra lại yêu cầu hoặc chuyển dược sĩ hỗ trợ.”
- **Người dùng thấy gì:**
  - **[Vị trí C]** Thẻ xem trước: người cần ghi nhận, mã hồ sơ dựng, nội dung “Khách tự báo dị ứng Ibuprofen” và mục đích “Ghi chú trong hồ sơ khách cho tư vấn sau”. Không ghi thành chẩn đoán đã được xác minh.
  - Nút “Sửa thông tin”, “Xác nhận lưu”, “Không lưu”. Yêu cầu ban đầu đã thể hiện ý muốn lưu; bước xác nhận dùng để kiểm tra đúng người/nội dung, không diễn giải rằng khách chưa từng đồng ý.
  - **[Vị trí D]** Nhãn “Chưa lưu” / “Đang lưu” / “Đã lưu” / “Lưu thất bại” / “Chưa xác định kết quả”, kèm mã yêu cầu và thời điểm từ kết quả tool mô phỏng. Lời AI nói không tự tạo được nhãn “Đã lưu”.
  - Khi tool báo lỗi: nút “Thử lại” và “Chuyển dược sĩ”. Khi kết quả chưa xác định do quá hạn: nút “Chuyển dược sĩ kiểm tra”, không tự gửi lại nhiều lần vì lần đầu có thể đã được lưu.
- **Người dùng làm gì tiếp:** Kiểm tra và sửa đúng người/nội dung, xác nhận hoặc hủy trước khi lưu. Khi lỗi rõ ràng, có thể thử lại; khi chưa xác định kết quả, chuyển người kiểm tra. Nếu khách không xác nhận thì không gọi tool và không lưu. Nếu khách báo đã lưu sai người, ngừng thao tác tự động và chuyển dược sĩ xử lý; không nói “đã xóa/đã sửa” vì bộ tool chưa có chức năng sửa hoặc xóa hồ sơ.
- **Ai được báo, sau bao lâu:** Không báo dược sĩ khi lưu thành công bình thường. Khi khách yêu cầu chuyển hoặc phát hiện lưu sai hồ sơ, gọi `chuyen_duoc_si` ngay và gửi mã yêu cầu, nội dung khách định lưu, hồ sơ đích, trạng thái kết quả và lỗi; chỉ gửi thông tin cần thiết cho xử lý ca. Mốc **10 giây mô phỏng** không có phản hồi được coi là “Chưa xác định kết quả”, không tự coi là thành công hoặc thất bại. Nếu chuyển cũng lỗi, nói rõ chưa chuyển được và giữ thông báo trạng thái; không hứa có chức năng cập nhật trong app mà track chưa cấp.
- **Quyết định và phương án đã loại:**
  - **Chọn:** Kiểm tra đúng người/nội dung, gọi `luu_ghi_chu_suc_khoe` sau xác nhận và dùng kết quả tool để quyết định thông báo; thất bại hoặc chưa xác định thì nói đúng trạng thái, cho đường phục hồi phù hợp.
  - **Loại báo đã lưu ngay khi nhận tin:** Ý định của khách chưa phải bằng chứng hành động đã thực hiện.
  - **Loại chỉ thêm câu “có thể chưa chính xác” sau thông báo thành công:** Vẫn khiến khách hiểu hồ sơ đã được cập nhật, không sửa được trạng thái dữ liệu.
  - **Loại chuyển dược sĩ cho mọi yêu cầu lưu:** Gây chờ không cần thiết khi yêu cầu rõ và tool hoạt động; dành chuyển người cho trường hợp sai hồ sơ, thất bại, chưa xác định kết quả hoặc khách yêu cầu.
  - **Loại tự gửi lại khi quá hạn:** Chưa biết thao tác trước đã thành công hay chưa; có thể tạo ghi chú trùng. Prototype không giả định có tool tra trạng thái hoặc chống trùng khi chưa khai báo.
- **Loại lỗi và cách sửa:** **Lỗi hệ thống** khi thông báo thành công không khớp kết quả lưu, hoặc dữ liệu vào sai hồ sơ. Hệ thống cần nhận trách nhiệm, nói rõ trạng thái thực tế và cung cấp cách phục hồi; không trách khách thiếu thông tin nếu hệ thống đã tự đoán. Khách sửa người cần lưu là thông tin hiệu chỉnh, không phải lý do để che lỗi thực thi.

### 3. Áp dụng 3 nguyên tắc HAX / PAIR

1. **HAX G10 — Thu hẹp phạm vi khi nghi ngờ/rủi ro:** **Vị trí A–B** ở #2 chặn gợi ý sản phẩm, hiện cảnh báo trước mọi yêu cầu bổ sung; **vị trí D** ở #8 không cho phép khẳng định thành công khi kết quả chưa xác định.
2. **HAX G9 — Sửa dễ dàng:** **Vị trí C** ở #8 có “Sửa thông tin” trước khi lưu. Nếu phát hiện sai sau lưu, có đường chuyển dược sĩ kèm mã yêu cầu, không hứa tự sửa/xóa bằng tool chưa có.
3. **PAIR — Explainability + Trust:** **Vị trí A** giải thích cảnh báo dựa vào lời khách; **vị trí C–D** cho thấy thông tin tự báo, hồ sơ đích và bằng chứng thực thi từ tool. Chọn trạng thái và căn cứ cụ thể thay vì phần trăm độ tin cậy không có cơ sở.

*Các ký hiệu A–D là vị trí thiết kế dự kiến; ở §5 cần gắn vào 1–2 ảnh prototype thực tế. Chưa có ảnh thì chưa coi là đã đáp ứng bằng chứng prototype của R5.*

### 4. Lỗi sửa câu chữ không cứu được và giải pháp thiết kế

- **Ở #2:** Dù câu chat có nhắc đi khám, việc vẫn hiển thị sản phẩm/nút mua tiếp tục dẫn khách vào luồng mua hàng. Khi nhận diện ca nguy hiểm theo fixture, thiết kế phải chặn `goi_y_san_pham`, không hiển thị nội dung gợi ý thuốc hoặc liều trong cả thẻ lẫn tin nhắn; giữ cảnh báo độc lập với kết quả chuyển dược sĩ. Trong prototype, đây là quy tắc luồng mô phỏng; không khẳng định bộ lọc từ khóa có thể nhận diện mọi ca thực tế.
- **Ở #8:** Câu “đã lưu” không cập nhật được hồ sơ. Nhãn thành công chỉ được tạo từ kết quả thành công của `luu_ghi_chu_suc_khoe` khớp hồ sơ và nội dung đã xác nhận; không tạo từ văn bản AI. Tool lỗi hoặc quá hạn phải đi nhánh riêng. Trước khi lưu cần kiểm tra đúng người; nếu đã lưu nhầm, phải chuyển người xử lý thay vì giả vờ có chức năng hoàn tác.
- **Giới hạn còn lại:** Thông tin dị ứng là do khách tự báo, chưa được xác minh chuyên môn. Lưu thành công không đồng nghĩa các lần tư vấn sau chắc chắn dùng đúng ghi chú; không hứa điều này khi fixture chưa cung cấp cơ chế đọc và kiểm tra hồ sơ cho lần sau.

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
