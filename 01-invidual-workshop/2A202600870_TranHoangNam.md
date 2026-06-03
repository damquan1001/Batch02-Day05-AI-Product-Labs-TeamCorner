# Workshop — Mổ App AI Thật

## Finding Note + Sketch As-is / To-be

**Sản phẩm phân tích:** MoMo — Moni
**AI feature:** Trợ thủ tài chính, phân tích chi tiêu, chatbot
**Thời gian phân tích:** 03/06/2026
**Hình thức:** Cá nhân dùng thử, sau đó chia sẻ theo nhóm

---

## 1. Sản phẩm được chọn

| Sản phẩm    | AI feature                                     | Cách truy cập |
| ----------- | ---------------------------------------------- | ------------- |
| MoMo — Moni | Trợ thủ tài chính, phân tích chi tiêu, chatbot | App MoMo      |

**Mục tiêu phân tích:**
Không đánh giá UI đẹp hay xấu. Mục tiêu là dùng Moni như một sản phẩm thật để tìm điểm gãy trong workflow thật, sau đó viết finding thành quyết định product có thể đưa vào SPEC.

---

## 2. Promise vs Reality

### 2.1. Product hứa gì?

Moni được định vị như một trợ thủ AI trong MoMo, có thể hỗ trợ người dùng hỏi nhanh, phân tích chi tiêu, gợi ý tài chính và hướng dẫn sử dụng các tính năng trong hệ sinh thái MoMo.

### 2.2. User nào được hứa sẽ được giúp?

User chính là người dùng MoMo phổ thông, đặc biệt là nhóm:

* Muốn xem lại chi tiêu cá nhân.
* Muốn hỏi nhanh về giao dịch, hóa đơn, vé máy bay, ngân hàng.
* Muốn được hướng dẫn thao tác trong app mà không phải tự tìm nhiều màn hình.
* Muốn AI giúp đi từ câu hỏi đến hành động cụ thể.

### 2.3. Kỳ vọng AI làm được task nào?

Khi dùng Moni, kỳ vọng AI có thể:

1. Hiểu câu hỏi tự nhiên của user.
2. Truy xuất hoặc tổng hợp dữ liệu chi tiêu cá nhân.
3. Hướng dẫn user đến đúng tính năng trong app.
4. Khi user muốn hành động, AI nên đưa shortcut, deeplink hoặc nút bấm.
5. Khi không chắc, AI nên hỏi lại hoặc đưa lựa chọn thay vì trả lời một chiều.
6. Khi không hỗ trợ trực tiếp, AI nên fallback về tính năng liên quan trong hệ sinh thái MoMo.

---

## 3. Evidence khi dùng thử

| Prompt / Input đã thử               | Hành vi quan sát được                                                                                                                                 | Evidence cần gắn      |
| ----------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- |
| “Liệt kê các khoản tiền vé máy bay” | Moni gom nhóm được các giao dịch liên quan, tổng hợp dữ liệu lịch sử và suy ra số tiền trung bình. Đây là happy path tốt.                             | `[Chèn screenshot 1]` |
| “đbagw ký”                          | Dù user gõ sai chính tả, Moni vẫn hiểu intent là “đăng ký”.                                                                                           | `[Chèn screenshot 2]` |
| “Cho nút đặt vé” / “Connect TPBank” | Moni trả lời bằng hướng dẫn dạng text, nhưng không có nút bấm hoặc deeplink để user đi thẳng đến màn hình cần thao tác.                               | `[Chèn screenshot 3]` |
| “Kiếm khách sạn gần đây”            | Moni báo giới hạn hỗ trợ, sau đó gợi ý user dùng các app bên ngoài như Booking, Agoda, Traveloka thay vì điều hướng về mini-app khách sạn trong MoMo. | `[Chèn screenshot 4]` |

---

## 4. Bốn paths cần phân tích

### 4.1. Happy Path

**Câu hỏi:** Khi AI đúng và tự tin, user thấy gì?

**Quan sát:**
Khi user hỏi về dữ liệu chi tiêu cá nhân như “liệt kê các khoản tiền vé máy bay”, Moni hiểu đúng intent, tìm được nhóm giao dịch liên quan và tổng hợp lại thành câu trả lời dễ đọc.

**User thấy:**

* Câu trả lời rõ ràng.
* Có dữ liệu cá nhân liên quan.
* Có tổng hợp, phân loại hoặc tính toán.
* User không cần tự vào lịch sử giao dịch để lọc thủ công.

**Kết luận:**
Happy path của Moni mạnh ở phần **data aggregation** và **natural language understanding** cho các câu hỏi tài chính hoặc lịch sử giao dịch.

---

### 4.2. Low-confidence Path

**Câu hỏi:** Khi AI không chắc, hệ thống có hỏi lại, show options hoặc chuyển người không?

**Quan sát:**
Ở các câu hỏi như “kiếm khách sạn gần đây”, Moni chưa thể hiện rõ trạng thái low-confidence. Thay vì hỏi lại user muốn “tìm khách sạn trong MoMo”, “tìm gần vị trí hiện tại”, hay “xem ưu đãi đặt phòng”, hệ thống trả lời theo hướng từ chối hoặc đẩy user ra ngoài app.

**Điểm gãy:**
AI không làm rõ intent mơ hồ. User có thể đang muốn:

* Tìm khách sạn gần vị trí hiện tại.
* Đặt phòng qua MoMo.
* Tìm ưu đãi khách sạn.
* Hỏi xem MoMo có hỗ trợ dịch vụ này không.

Nhưng Moni không hỏi lại và cũng không đưa lựa chọn.

**Path còn thiếu trong product:**
Moni chưa có low-confidence path rõ ràng cho các intent nằm sát ranh giới giữa “ngoài scope AI” và “có thể điều hướng sang dịch vụ khác trong MoMo”.

---

### 4.3. Failure Path

**Câu hỏi:** Khi AI sai, user biết bằng cách nào và sửa thế nào?

**Quan sát:**
Failure rõ nhất xuất hiện ở hai nhóm tình huống:

1. User muốn hành động ngay, nhưng AI chỉ trả lời bằng text.
2. User hỏi dịch vụ có thể liên quan đến hệ sinh thái MoMo, nhưng AI lại gợi ý nền tảng bên ngoài.

**Ví dụ failure 1:**
User hỏi “cho nút đặt vé” hoặc “connect TPBank”.
Moni hiểu được nhu cầu nhưng không đưa nút bấm, không mở màn hình đích, không có deeplink.

**Impact:**
User phải tự nhớ hướng dẫn, thoát chat, tự tìm màn hình trong app. Workflow bị gãy ở bước chuyển từ “hiểu intent” sang “thực thi hành động”.

**Ví dụ failure 2:**
User hỏi “kiếm khách sạn gần đây”.
Moni không điều hướng về dịch vụ khách sạn trong MoMo mà lại nhắc tới Booking, Agoda, Traveloka.

**Impact:**
MoMo có nguy cơ mất user sang nền tảng khác. Đây không chỉ là lỗi UX mà còn là lỗi logic kinh doanh và retention.

---

### 4.4. Correction Path

**Câu hỏi:** Khi user sửa, correction có được lưu/log/học lại không hay biến mất?

**Quan sát:**
Trong trải nghiệm hiện tại, chưa thấy Moni có cơ chế rõ ràng để user sửa câu trả lời hoặc báo rằng AI đã điều hướng sai. Ví dụ, nếu Moni gợi ý Booking/Agoda nhưng user muốn đặt khách sạn trong MoMo, user không thấy lựa chọn như:

* “Ý tôi là đặt trong MoMo”
* “Không đúng”
* “Cho tôi mở tính năng khách sạn của MoMo”
* “Ghi nhớ lần sau ưu tiên dịch vụ trong MoMo”

**Path còn thiếu trong product:**
Correction path chưa rõ. User có thể sửa bằng cách nhập lại câu khác, nhưng correction đó chưa được thể hiện là có lưu, log hoặc cải thiện luồng sau.

**Kết luận:**
Moni cần một correction path có cấu trúc, ít nhất ở mức UX: user có thể đánh dấu câu trả lời sai, chọn intent đúng, và hệ thống điều hướng lại ngay.

---

## 5. Finding viết thành Product Decision

### Finding 1 — Actionless AI làm gãy workflow sau khi đã hiểu đúng intent

**Không viết:**
Bot trả lời dài dòng, không có nút.

**Viết thành finding:**
Khi user hỏi các câu có intent hành động như “cho nút đặt vé” hoặc “connect TPBank”, AI hiểu được nhu cầu nhưng product chỉ trả lời bằng hướng dẫn text, hậu quả là user phải tự nhớ bước, rời khỏi khung chat và tự tìm tính năng trong app. Lỗi thuộc layer **UX Recovery + Data/Tool Integration**. Nên sửa bằng requirement: mọi intent có thể thực thi trong MoMo phải trả về text ngắn kèm **deeplink/action button** mở thẳng màn hình đích.

**Product decision:**
Moni không nên dừng ở vai trò chatbot hướng dẫn. Với các intent đã map được vào tính năng trong MoMo, Moni phải hoạt động như một **action-oriented assistant**.

---

### Finding 2 — Fallback sai làm mất user khỏi hệ sinh thái MoMo

**Không viết:**
Bot ngu vì gợi ý app đối thủ.

**Viết thành finding:**
Khi user hỏi “kiếm khách sạn gần đây”, AI nhận diện đây là nhu cầu ngoài phạm vi trả lời trực tiếp nhưng lại fallback bằng cách gợi ý Booking, Agoda, Traveloka, hậu quả là user bị đẩy ra khỏi hệ sinh thái MoMo dù MoMo có thể có dịch vụ/mini-app liên quan đến đặt phòng. Lỗi thuộc layer **Promise + Safety/Business Boundary + UX Recovery**. Nên sửa bằng fallback rule: khi intent nằm ngoài khả năng xử lý trực tiếp, AI phải kiểm tra dịch vụ liên quan trong hệ sinh thái MoMo trước, sau đó mới trả lời giới hạn.

**Product decision:**
Fallback của Moni phải ưu tiên giữ user trong hệ sinh thái MoMo, không đề xuất đối thủ nếu MoMo có tính năng tương đương hoặc liên quan.

---

## 6. Sketch As-is / To-be

### 6.1. Flow 1 — User muốn hành động trong MoMo

| As-is                                                | To-be                                                         |
| ---------------------------------------------------- | ------------------------------------------------------------- |
| User nhập: “Cho nút đặt vé” hoặc “Connect TPBank”    | User nhập: “Cho nút đặt vé” hoặc “Connect TPBank”             |
| Moni hiểu intent                                     | Moni hiểu intent                                              |
| Moni trả lời bằng đoạn text hướng dẫn nhiều bước     | Moni trả lời text ngắn                                        |
| User phải ghi nhớ hướng dẫn                          | Moni hiển thị nút: “Mở đặt vé máy bay” hoặc “Liên kết TPBank” |
| User thoát chat                                      | User bấm nút ngay trong chat                                  |
| User tự tìm tính năng trong app                      | App mở thẳng màn hình đích                                    |
| **Điểm gãy:** AI hiểu nhưng không giúp user thực thi | **Path đã sửa:** AI chuyển từ hiểu intent sang action         |

**As-is text flow:**
`User hỏi hành động`
→ `Moni hiểu intent`
→ `Trả lời hướng dẫn text`
→ `User tự tìm tính năng`
→ `Rớt workflow`

**To-be text flow:**
`User hỏi hành động`
→ `Moni hiểu intent`
→ `Text ngắn + action button`
→ `User bấm nút`
→ `Mở đúng màn hình đích`

---

### 6.2. Flow 2 — User hỏi dịch vụ nằm gần ngoài scope

| As-is                                            | To-be                                                      |
| ------------------------------------------------ | ---------------------------------------------------------- |
| User nhập: “Kiếm khách sạn gần đây”              | User nhập: “Kiếm khách sạn gần đây”                        |
| Moni xử lý intent                                | Moni xử lý intent                                          |
| Moni báo giới hạn hoặc nói không chuyên mảng này | Moni nhận diện intent mơ hồ / ngoài scope trực tiếp        |
| Moni gợi ý Booking, Agoda, Traveloka             | Moni hỏi lại hoặc đưa options                              |
| User có thể rời MoMo                             | Option 1: “Tìm ưu đãi khách sạn trong MoMo”                |
| **Điểm gãy:** fallback đẩy user sang đối thủ     | Option 2: “Mở mini-app đặt khách sạn”                      |
|                                                  | Option 3: “Tôi cần tìm gần vị trí hiện tại”                |
|                                                  | **Path đã sửa:** fallback giữ user trong hệ sinh thái MoMo |

**As-is text flow:**
`User hỏi khách sạn`
→ `Moni không xử lý được đầy đủ`
→ `Gợi ý app bên ngoài`
→ `Mất user khỏi MoMo`

**To-be text flow:**
`User hỏi khách sạn`
→ `Moni nhận diện low-confidence`
→ `Hỏi lại / show 2-3 options`
→ `Ưu tiên mini-app hoặc dịch vụ liên quan trong MoMo`
→ `User tiếp tục trong MoMo`

---

## 7. Requirement đề xuất

### Requirement 1 — Deeplink cho action intent

Nếu intent của user match với một tính năng có sẵn trong MoMo, Moni phải trả về tối thiểu:

* Một câu trả lời ngắn.
* Một nút CTA chính.
* Deeplink mở thẳng màn hình đích.
* Không chỉ trả lời bằng hướng dẫn text nếu có thể thực thi bằng app.

**Ví dụ:**
User: “Connect TPBank”
Moni nên trả lời:
“Bạn có thể liên kết TPBank tại đây.”
CTA: “Mở liên kết ngân hàng”

---

### Requirement 2 — Low-confidence path cho intent mơ hồ

Nếu AI không chắc user muốn gì, Moni không nên đoán một hướng duy nhất. Moni nên hỏi lại hoặc show options.

**Ví dụ:**
User: “Kiếm khách sạn gần đây”
Moni nên trả lời:
“Bạn muốn tìm theo hướng nào?”

Options:

1. “Xem ưu đãi khách sạn trong MoMo”
2. “Mở tính năng đặt khách sạn”
3. “Tìm khách sạn gần vị trí hiện tại”

---

### Requirement 3 — Business-safe fallback

Khi user hỏi dịch vụ ngoài scope trực tiếp, Moni phải kiểm tra xem MoMo có dịch vụ liên quan không trước khi gợi ý app bên ngoài.

**Rule đề xuất:**
Không đề xuất Booking, Agoda, Traveloka nếu MoMo có mini-app hoặc dịch vụ đặt phòng liên quan.

---

### Requirement 4 — Correction path

Moni cần có cách để user sửa khi AI hiểu sai.

**Ví dụ UX:**
Sau mỗi câu trả lời có thể có lựa chọn:

* “Không đúng ý tôi”
* “Tôi muốn làm việc này trong MoMo”
* “Mở tính năng liên quan”
* “Báo câu trả lời sai”

Correction nên được log lại để cải thiện intent mapping và fallback rule.

---

## 8. Self-check trước khi nộp

* [x] Có observation cụ thể từ quá trình dùng thử.
* [x] Có chỗ để gắn ít nhất 1 screenshot.
* [x] Có đủ 4 paths: Happy, Low-confidence, Failure, Correction.
* [x] Có nói rõ path nào hiện chưa rõ hoặc chưa có trong product.
* [x] Finding được viết thành product decision, không chỉ là nhận xét.
* [x] Sketch có cả as-is và to-be.
* [x] Có câu nói rõ finding này sẽ đổi gì trong SPEC.

---

## 9. Câu SPEC cần thay đổi

Finding này sẽ đổi SPEC của Moni từ “AI trả lời và hướng dẫn bằng text” thành “AI hiểu intent, xác định độ tự tin, ưu tiên action button/deeplink cho task có thể thực thi, dùng low-confidence path khi không chắc, và fallback về dịch vụ trong hệ sinh thái MoMo trước khi gợi ý lựa chọn bên ngoài.”
