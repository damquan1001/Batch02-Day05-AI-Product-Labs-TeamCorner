# Thin SPEC — VinmecCare · AI Agent đặt lịch (symptom → form pre-fill)

Thin SPEC không phải PRD đầy đủ. Đây là bản cam kết đủ rõ để sáng Day 06 nhóm build ngay.

## 1. Track, product/app và user

**Track:** Healthcare  
**Product/app thật (tham chiếu):** Trợ lý ảo VinmecCare + form [Đăng ký khám](http://vinmec.com/vie/dang-ky-kham/)  
**Prototype Day 06:** Chatbot **AI agent** (mock Vinmec) — không tích hợp VinBigdata production  
**User cụ thể:** Người bệnh lần đầu mô tả triệu chứng bằng tiếng Việt, không rành chọn khoa/bác sĩ  
**Nhóm có phải user thật không?** Đã self-use VinmecCare; prototype giả lập flow tốt hơn as-is thật  

## 2. Evidence summary

| Evidence | Nguồn | User/pain nói lên điều gì? | SPEC phải đổi gì? |
|---|---|---|---|
| Chat: triệu chứng → link, không gợi ý khoa | Screenshot self-use | Cần agent map symptom → khoa/bác sĩ | LLM + mock catalog |
| Form web nhiều field, user tự chọn | vinmec.com/vie/dang-ky-kham/ | Ma sát cao, dễ bỏ giữa chừng | Pre-fill phần đặt lịch (không PII) |
| Handoff mất context | Chat Vinmec thật | Cần chuyển state sang form | Object `bookingDraft` từ agent → form |

## 3. Pain statement

```text
User mô tả triệu chứng và muốn đặt lịch qua chat,
gặp khó vì bot thật chỉ gửi link web và form bắt tự chọn khoa/bác sĩ,
dẫn tới bỏ giữa chừng hoặc chọn sai.
Bằng chứng: screenshot VinmecCare + form đăng ký khám.
```

## 4. Build slice

```text
Cho người bệnh lần đầu đang nhập triệu chứng và thông tin đặt lịch không nhạy cảm (tuổi/năm sinh, khu vực/cơ sở, triệu chứng, lý do khám),
prototype là chatbot AI agent dùng LLM + data mock (cơ sở Vinmec, chuyên khoa, bác sĩ, slot giờ gợi ý)
để hỏi làm rõ, gợi ý 2–3 khoa, user chọn khoa/bác sĩ/thời gian,
tạo ra form đặt lịch đã điền sẵn các field hành chính/y khoa (read-only hoặc editable),
user chỉ nhập thông tin cá nhân (họ tên, SĐT, ngày sinh, giới tính, email…)
và submit — dữ liệu cá nhân KHÔNG đi qua LLM (chỉ client → backend mock / local state),
và xử lý failure mode (gợi ý khoa sai / red flag) bằng low-confidence + hotline mock.
```

### Kiến trúc prototype (ranh giới dữ liệu)

```text
┌─────────────────────────────────────────────────────────────┐
│  PHASE A — Chat (AI agent, có LLM)                          │
│  Input user: triệu chứng, tuổi, cơ sở/khu vực, (không PII)  │
│  Tool/mock DB: hospitals, specialties, doctors, slots       │
│  Output: bookingDraft { facility, specialty, doctor?,       │
│           suggestedDate?, reasonVisit, symptomSummary }     │
└──────────────────────────┬──────────────────────────────────┘
                           │ handoff (JSON/state, không qua prompt)
┌──────────────────────────▼──────────────────────────────────┐
│  PHASE B — Form đặt lịch (KHÔNG LLM)                        │
│  Pre-filled: cơ sở, chuyên khoa, bác sĩ, thời gian, lý do   │
│  User nhập: họ tên, SĐT, ngày sinh, giới tính, email, OTP*  │
│  Submit → mock API / console log (demo)                       │
└─────────────────────────────────────────────────────────────┘
* OTP mock hoặc bỏ qua trong demo 1 ngày
```

**Nguyên tắc privacy:** Họ tên, SĐT, email, CMND… **không** gửi vào prompt LLM; không log PII trong transcript agent. Chỉ `bookingDraft` + field form Phase B.

### Mock data (Day 06)

| Dataset | Nội dung tối thiểu |
|---------|-------------------|
| `facilities` | 2–3 cơ sở HN (Times City, Smart City…) |
| `specialties` | 5–8 khoa (Tiêu hóa, Nội tổng quát, Cấp cứu flag…) |
| `doctors` | 2–3 BS / khoa (optional chọn) |
| `slots` | 3 ngày gợi ý (giống UI Vinmec) |
| `redFlags` | Rule/keyword → chặn gợi ý khoa, show hotline |

### AI decision (một việc cụ thể)

Map **triệu chứng + tuổi + cơ sở** → rank **2–3 chuyên khoa** (có `reason` ngắn); user **chọn** khoa → agent gợi ý bác sĩ/slot từ mock.

## 5. Auto/Aug decision

- [x] **Augmentation:** AI gợi ý khoa/BS/slot; user quyết và sửa trên form.
- [x] **Conditional automation (một phần):** Agent tự điền form Phase B từ `bookingDraft`; Phase B submit không qua LLM.
- [ ] **Automation:** Không — không auto-submit đặt lịch; không auto-điền PII.

**Lý do:** Rủi ro y tế + privacy; PII tách khỏi LLM là requirement prototype.  
**Human role:** decider (chọn khoa/BS) + nhập PII trên form + rescuer (red flag → hotline)  

## 6. Four paths

| Path | Prototype phải thể hiện gì? |
|---|---|
| Happy | Triệu chứng → agent hỏi tối đa 2 câu → 2–3 khoa → chọn khoa + BS/slot mock → form pre-fill → user điền PII → submit mock thành công |
| Low-confidence | Confidence thấp → hỏi lại / 2 nhánh khoa; hoặc user đổi cơ sở trên form pre-fill |
| Failure | "Khoa không đúng" → chọn lại từ mock list; hoặc red flag → không mở form, chỉ hotline |
| Correction | Đổi khoa/BS trên form pre-fill trước submit; chat không cần gửi lại PII vào LLM |

## 7. Failure mode nguy hiểm nhất

```text
Nếu user mô tả triệu chứng nặng (red flag),
AI có thể gợi ý nhầm khoa ngoại trú,
hậu quả là trễ điều trị.
Prototype: rule red-flag trước LLM → chặn bookingDraft → cảnh báo + hotline;
không chuyển sang form đặt lịch.
Owner test: Khoa (2A202600922).
```

**Failure privacy:** Nếu user paste SĐT/họ tên vào chat → agent từ chối lưu vào prompt / nhắc nhập ở form Phase B.

## 8. Owner plan cho sáng Day 06

| Thành viên | Việc phụ trách | Deliverable |
|---|---|---|
| Khoa (2A202600922) | SPEC + mock JSON (khoa, BS, cơ sở) + red-flag rules | `mock-data/` hoặc embed trong repo |
| [Thành viên 2] | Chat UI + LLM agent (tool/mock lookup) | Flow Phase A |
| [Thành viên 3] | Form pre-fill + submit (no LLM) | Flow Phase B + privacy boundary |
| [Thành viên 4] | Test script + demo 3 phút | Happy / low-confidence / red flag / PII không vào chat |

### Demo script (3 phút)

1. Nhập triệu chứng (không nhập họ tên/SĐT trong chat).  
2. Agent gợi ý khoa → chọn → form mở đã có khoa/BS/ngày.  
3. Điền PII trên form → submit.  
4. (Optional) Thử red flag → không có form, có hotline.
