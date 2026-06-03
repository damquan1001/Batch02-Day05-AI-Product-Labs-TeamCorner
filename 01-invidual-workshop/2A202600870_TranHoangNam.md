# BÁO CÁO PHÂN TÍCH UX & TỐI ƯU HÓA SẢN PHẨM AI
**Đối tượng phân tích:** Trợ thủ AI MoMo Moni
**Thời gian phân tích:** 03/06/2026

---

## 1. TỔNG QUAN TRẢI NGHIỆM (EMPATHY & TESTING)
Thông qua việc thử nghiệm các truy vấn thực tế, hệ thống Moni bộc lộ rõ cả những năng lực xuất sắc trong xử lý dữ liệu lẫn những lỗ hổng nghiêm trọng trong logic nghiệp vụ và định hướng hành vi người dùng (User Journey).

### 1.1. Điểm sáng trải nghiệm (Happy Path)
*   **Truy vấn & Tổng hợp dữ liệu cá nhân (Data Aggregation):** Moni thực hiện xuất sắc việc gom nhóm dữ liệu lịch sử ("liệt kê các khoản tiền vé máy bay"), tính toán tổng số tiền, và nội suy ra con số trung bình mỗi ngày.
*   **Dung sai lỗi chính tả (Typo Tolerance):** NLP của hệ thống có khả năng tự sửa lỗi (Autocorrect) tốt. Khi người dùng gõ sai "đbagw ký", AI vẫn bắt đúng Intent (Ý định) là "đăng ký".

### 1.2. Các Điểm gãy trải nghiệm (Failure Paths)
*   **Thiếu khả năng thực thi (Actionless):** Người dùng yêu cầu các phím tắt (shortcut) để hành động ngay ("cho nút đăng ký", "làm sao để connect TPBank"). Tuy nhiên, AI phản hồi bằng văn bản thuần túy (Text-heavy), ép người dùng phải tự ghi nhớ các bước và tự điều hướng tay, tạo ra ma sát lớn.
*   **Xung đột hệ sinh thái (Ecosystem Conflict):** Ở truy vấn "kiếm khách sạn gần đây", hệ thống không chỉ báo lỗi (thiếu dữ liệu Location-based) mà còn mắc sai lầm chí mạng về mặt kinh doanh: **Đẩy người dùng sang nền tảng đối thủ cạnh tranh** (Booking, Agoda, Traveloka), bỏ qua hoàn toàn mini-app Đặt phòng Khách sạn đang có sẵn trên hệ sinh thái MoMo.

---

## 2. PHÂN TÍCH LUỒNG TRẢI NGHIỆM (AS-IS VS. TO-BE FLOWS)

Dưới đây là sơ đồ luồng phân tích bóc tách các điểm nghẽn hiện tại và đề xuất giải pháp thiết kế lại (Redesign), sử dụng luồng mũi tên trực quan (Text-based Flow).

### 2.1. Luồng Yêu cầu Thực thi / Điều hướng (Ví dụ: Đặt vé máy bay, Liên kết ngân hàng)

**🔴 AS-IS: Luồng hiện tại (Thiếu thực thi)**
`[Người dùng nhập query: 'Cho nút đặt vé' / 'Connect TPBank']`
 ➔ `[Moni xử lý NLP]`
 ➔ `[Moni trả về đoạn text dài hướng dẫn các bước manual]`
 ➔ `[Người dùng phải ghi nhớ các bước]`
 ➔ `[Thoát khung chat AI]`
 ➔ `[Tự tìm kiếm tính năng trên App]`
 ➔ **[❌ ĐIỂM MA SÁT / RỚT USER]**

**🟢 TO-BE: Luồng đề xuất (Hành động tức thì)**
`[Người dùng nhập query: 'Cho nút đặt vé' / 'Connect TPBank']`
 ➔ `[Moni xử lý NLP]`
 ➔ `[Moni trả lời text ngắn + NÚT BẤM DEEPLINK]`
 ➔ `[Người dùng bấm nút trực tiếp trong chat]`
 ➔ **[✅ MỞ THẲNG MÀN HÌNH TÍNH NĂNG ĐÍCH]**

---

### 2.2. Luồng Yêu cầu Dịch vụ chéo / Nằm ngoài scope (Ví dụ: Tìm khách sạn)

**🔴 AS-IS: Luồng hiện tại (Đẩy user cho đối thủ)**
`[Người dùng: 'Tìm khách sạn gần đây']`
 ➔ `[Moni xử lý NLP]`
 ➔ `[Báo lỗi: Chỉ chuyên mảng ăn uống]`
 ➔ `[Gợi ý user dùng app Booking, Agoda...]`
 ➔ **[❌ MẤT KHÁCH HÀNG / GIẢM RETENTION]**

**🟢 TO-BE: Luồng đề xuất (Giữ chân & Bán chéo)**
`[Người dùng: 'Tìm khách sạn gần đây']`
 ➔ `[Moni xử lý NLP]`
 ➔ `[Thừa nhận giới hạn: Chưa hỗ trợ tìm theo bản đồ hiện tại]`
 ➔ `[Pivot/Cross-sell: 'Nhưng MoMo có hỗ trợ đặt phòng giá tốt...']`
 ➔ `[Kèm NÚT BẤM mở mini-app Đặt khách sạn của MoMo]`
 ➔ **[✅ GIỮ CHÂN USER & TĂNG DOANH THU CHÉO]**

---

## 3. QUYẾT ĐỊNH SẢN PHẨM (PRODUCT DECISION)

Dựa trên quá trình "mổ xẻ" các luồng (path) yếu nhất, quyết định sản phẩm chiến lược cần được triển khai ngay lập tức bao gồm 2 mũi nhọn:

1.  **Dịch chuyển từ "Chatbot Đọc Text" sang "Trợ lý Thực thi" (Action-Oriented Assistant):** Bắt buộc tích hợp kiến trúc **Deeplink/Action Buttons** vào hệ thống phản hồi của AI. Mọi câu hỏi mang tính chất "Làm sao để...", "Mở cho tôi...", "Đăng ký..." phải được kết thúc bằng một nút bấm dẫn thẳng đến giao diện xử lý (UI), tuyệt đối không để người dùng tự "bơi" trong app.
2.  **Đóng lỗ hổng Logic Cạnh tranh (Competitive Business Logic):** Rà soát lại toàn bộ bộ lọc câu trả lời (Safety/Boundary Filters) của AI. Cấm tuyệt đối hành vi AI tự động đề xuất người dùng sử dụng ứng dụng đối thủ (Agoda, Booking, Traveloka). Thay vào đó, thiết lập cơ chế **Fallback Navigation**: Khi người dùng hỏi một dịch vụ ngoài vùng lõi (như khách sạn), AI phải lập tức quét hệ sinh thái nội bộ (Mini-apps) để giới thiệu sản phẩm liên quan của chính công ty.
