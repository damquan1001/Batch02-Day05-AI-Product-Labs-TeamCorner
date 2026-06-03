# Evidence Pack — Team Corner · VinmecCare

Nộp kèm thin SPEC cuối Day 05.

## 1. Nhóm và track

**Tên nhóm:** Team Corner (Batch02-Day05-AI-Product-Labs-TeamCorner)  
**Track:** Healthcare — bệnh viện / đặt lịch khám  
**Product/app đã chọn (tham chiếu):** Trợ lý ảo VinmecCare (Vinmec, VinBigdata)  
**Build slice Day 06 (đã chốt):** Chatbot **AI agent** — triệu chứng + input không PII → LLM + **mock** (khoa, bác sĩ, cơ sở, slot) → **form pre-fill** (đặt lịch) → user chỉ nhập **thông tin cá nhân** → submit **không qua LLM**  

## 2. Self-use evidence

| Observation | Screenshot/link | Path liên quan | Điều học được |
|---|---|---|---|
| User mô tả triệu chứng + hẹn lịch; bot hỏi tuổi, không map → khoa | `assets/vinmec-chat-01-trien-chung.png` | Happy (một phần) | Cần agent + mock specialty |
| User "2004"; bot hỏi cơ sở free text | `assets/vinmec-chat-02-tuoi-co-so.png` | Low-confidence | Mock facilities + chips thay free text |
| Bot chỉ link + hotline | `assets/vinmec-chat-03-link-hotline.png` | Failure / handoff | Thay link bằng form pre-fill trong prototype |
| Form web: cơ sở, khoa, BS, thời gian, rồi PII riêng | `assets/vinmec-form-dang-ky-kham.png` | — | Tách Phase A (LLM) vs Phase B (PII, no LLM) |

## 3. User / review / social evidence

| Quote / review / observation | Nguồn | User là ai? | Pain/failure mode |
|---|---|---|---|
| "Chatbot chỉ gửi link, phải tự điền form dài" | Self-use (03/06/2026) | Người đặt khám Vinmec | Handoff / abandonment |
| "Không biết đau bụng đi khoa nào" | Prompt thử nhóm | Người lần đầu | Cần AI gợi ý từ symptom |
| Lo ngại nhập SĐT/họ tên vào chat AI | Insight nhóm (privacy) | Mọi user | PII chỉ trên form, không LLM |

```text
Review store: bổ sung 2 quote trước M1 Day 06.
```

## 4. Competitor / analog evidence

| App / mô hình | Họ xử lý thế nào? | Pattern cho prototype | 1 ngày? |
|---|---|---|---|
| Form Vinmec | Dropdown khoa, BS, ngày; PII section riêng | Pre-fill section đặt lịch; PII user tự điền | Có |
| MyVinmec | Wizard có cấu trúc | Agent = wizard trong chat | Có (mock) |
| Best practice AI + form | Tách inference vs transactional submit | `bookingDraft` → form, PII bypass LLM | Có |

## 5. Evidence -> Insight

```text
Evidence nổi bật:
- Chat thật: symptom không thành khoa/BS; chỉ link.
- Form thật: nhiều field; PII tách section — phù hợp Phase B không LLM.

Insight:
User cần một luồng: mô tả bệnh → được gợi ý khoa/BS → form gần xong,
không phải nhập lại từ đầu và không phải đưa SĐT/họ tên vào chat AI.

Opportunity:
AI agent + mock catalog trong chat;
handoff sang form đã fill cơ sở/khoa/BS/lý do khám;
chỉ PII trên form, submit ngoài LLM.
```

## 6. Evidence đổi SPEC như thế nào?

- [x] Đổi build slice (từ "chỉ gợi ý khoa" → **agent + pre-fill form + privacy boundary**).
- [x] Đổi kiến trúc (2 phase: LLM / no-LLM).
- [x] Đổi deliverable Day 06 (chat + form + mock JSON).
- [ ] Đổi user chính (giữ nguyên).

```text
Trước: symptom → gợi ý 2–3 khoa → summary → link.
Sau: symptom → AI agent (mock khoa/BS/slot) → form pre-fill → user PII → submit (no LLM).
Lý do: giải quyết trọn handoff + giảm rủi ro lộ PII; vẫn demo được 1 ngày với mock.
```
