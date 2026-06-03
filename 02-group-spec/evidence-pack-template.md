# Evidence Pack — Team Corner · VinmecCare

Nộp kèm thin SPEC cuối Day 05.

## 1. Nhóm và track

**Tên nhóm:** Team Corner (Batch02-Day05-AI-Product-Labs-TeamCorner)  
**Track:** Healthcare — bệnh viện / đặt lịch khám  
**Product/app đã chọn:** Trợ lý ảo VinmecCare (chat website/app Vinmec, Powered by VinBigdata)  
**Build slice đang nghĩ:** Từ mô tả triệu chứng → AI gợi ý 2–3 chuyên khoa + user xác nhận → bản tóm tắt trước khi sang form đặt lịch  

## 2. Self-use evidence

Nhóm tự dùng app/workflow và ghi lại điểm gãy.

| Observation | Screenshot/link | Path liên quan | Điều học được |
|---|---|---|---|
| User mô tả triệu chứng + "hẹn lịch giúp tôi"; bot hỏi tuổi/năm sinh, chưa map triệu chứng → khoa | `assets/vinmec-chat-01-trien-chung.png` | Happy (một phần) | Bot thu slot cơ bản, không giải quyết intent "chọn khoa" |
| User trả "2004"; bot hỏi tên cơ sở Vinmec (list dài, free text) | `assets/vinmec-chat-02-tuoi-co-so.png` | Low-confidence | User có thể trả mơ hồ; bot chưa có structured choice cơ sở |
| User: "Vin ở hà nội"; bot gửi link đặt lịch + hotline, không đặt trong chat | `assets/vinmec-chat-03-link-hotline.png` | Failure / handoff | Handoff mất context triệu chứng; user phải tự hoàn tất trên web |
| Form [Đăng ký khám](http://vinmec.com/vie/dang-ky-kham/) bắt buộc chọn cơ sở + chuyên khoa (+ bác sĩ, thời gian, OTP) | `assets/vinmec-form-dang-ky-kham.png` | — | Gap: chat thu symptom, web thu structured fields — user tự chọn khoa |

## 3. User / review / social evidence

| Quote / review / observation | Nguồn | User là ai? | Pain/failure mode |
|---|---|---|---|
| "Chatbot chỉ gửi link, phải tự điền form dài" | Self-use nhóm (03/06/2026) | Người đặt khám qua web Vinmec | Handoff / abandonment |
| "Không biết đau bụng nên đi khoa nào" | Prompt thử: đau bụng, táo bón, không đi được | Người lần đầu / ít am hiểu y khoa | Intent → specialty mapping thiếu |
| "Muốn hẹn nhanh trong chat nhưng phải gọi tổng đài" | Response bot (danh sách hotline) | User gấp / hạn chế di chuyển | Không automation trong chat |

```text
Review App Store/MyVinmec: nhóm bổ sung 2 quote thật trước checkpoint M1 Day 06.
Giả định bổ sung hiện tại. Nhóm sẽ kiểm bằng tìm review MyVinmec trên store + 1 phỏng vấn nhanh (2 phút) trước M1 Day 06.
```

## 4. Competitor / analog evidence

| App / mô hình tham khảo | Họ xử lý task này thế nào? | Pattern học được | Có áp dụng trong 1 ngày không? |
|---|---|---|---|
| MyVinmec App | Wizard đặt lịch: cơ sở → chuyên khoa → (bác sĩ) → thời gian | Structured flow + chọn từ danh sách | Có — mock UI chọn khoa trong chat |
| Form web Vinmec | Dropdown bắt buộc chuyên khoa | Biết field bắt buộc prototype phải pre-fill/gợi ý | Có — output summary map sang form |
| Chatbot y tế triage (analog) | Hỏi thêm + gợi ý khoa + red flag | Low-confidence + escalation | Có — rule + LLM gợi ý 2–3 khoa |

## 5. Evidence -> Insight

```text
Evidence nổi bật nhất:
- Chat VinmecCare thu triệu chứng + tuổi + cơ sở rồi chuyển link, không gợi ý chuyên khoa.
- Form web bắt buộc user tự chọn chuyên khoa/bác sĩ.
- User trả địa điểm mơ hồ ("Hà Nội") không được làm rõ trước handoff.

Insight:
User đặt khám lần đầu / mô tả bệnh bằng lời không chỉ cần "link đặt lịch".
Họ cần hỗ trợ quyết định chuyên khoa an toàn, ít bước,
vì chat và form web không nối được symptom → specialty.

Opportunity:
AI có thể augment bằng cách hỏi tối đa 2 câu làm rõ + gợi ý 2–3 chuyên khoa (có lý do ngắn),
user chọn và xác nhận, rồi mới handoff sang form/link hoặc hotline.
```

## 6. Evidence đổi SPEC như thế nào?

- [ ] Đổi user chính. (giữ: người lần đầu đặt khám qua chat)
- [x] Đổi pain statement.
- [x] Đổi build slice.
- [x] Đổi Auto/Aug decision.
- [x] Đổi 4 paths.
- [x] Đổi failure mode.
- [x] Đổi owner/test plan.

```text
Trước evidence, nhóm định "cải thiện chatbot đặt lịch Vinmec".
Sau evidence, nhóm đổi thành "prototype: symptom → gợi ý 2–3 chuyên khoa + xác nhận + summary",
không build OTP/API đặt lịch thật.
Lý do: chat hiện chỉ handoff link; task gãy rõ nhất ở chọn khoa; demo được trong 1 ngày.
```
