# Toolkit — Từ Evidence Đến Build Slice

Dùng sau khi nhóm đã có evidence. Mục tiêu là chốt một build slice đủ nhỏ cho Day 06.

## 1. Gom evidence thành cụm

Gom theo **workflow/pain**, không gom theo tên feature.

Ví dụ cụm tốt:

- "Không biết chọn chuyên khoa"
- "Không hiểu vì sao bị tính phí"
- "Muốn sửa output nhưng không có chỗ sửa"
- "Bot trả lời tự tin nhưng không dẫn nguồn"

## 2. Viết insight

Form:

```text
User [segment] không chỉ cần [surface need].
Họ thật ra cần [deeper need],
vì [evidence pattern].
```

Ví dụ:

```text
Người lần đầu đi khám không chỉ cần danh sách chuyên khoa.
Họ cần hỗ trợ ra quyết định an toàn,
vì nhiều review/observation cho thấy họ không biết triệu chứng của mình nên đi khoa nào.
```

## 3. Viết opportunity

Form:

```text
Cơ hội là dùng AI để [augment/automate hành động hẹp],
giúp user [kết quả],
trong khi vẫn kiểm soát [failure/risk].
```

## 4. Chọn build slice

Build slice tốt phải qua 5 câu hỏi:

| Câu hỏi | Đạt khi |
|---|---|
| User cụ thể chưa? | Nói được ai dùng, trong bối cảnh nào. |
| Task đủ hẹp chưa? | Demo được trong 3-5 phút. |
| AI decision rõ chưa? | AI gợi ý/tự làm một việc cụ thể. |
| Failure path rõ chưa? | Có một case AI không chắc hoặc sai để test. |
| Có evidence không? | Có bằng chứng từ self-use/review/user/competitor. |

## 5. Quyết định: giữ, giảm scope, hay đổi hướng?

| Tình huống | Quyết định |
|---|---|
| Evidence yếu, user mơ hồ | Dừng build sâu; quay lại research 20 phút. |
| Ý tưởng quá rộng | Giữ domain, cắt xuống một flow. |
| AI không cần thiết | Dùng rule/manual prototype; ghi rõ vì sao không dùng AI sâu. |
| Rủi ro cao | Chọn augmentation hoặc conditional automation. |
| Không demo được trong 1 ngày | Đưa phần lớn vào backlog, giữ một path nhỏ. |

## 6. Câu chốt cuối

Điền câu này trước khi rời lớp:

```text
Dựa trên [evidence],
nhóm sẽ build [prototype slice],
cho [user],
để giải quyết [pain],
bằng cách AI [augment/automate task],
và sẽ test failure path [failure mode].
```

## 7. Backlog

Những thứ **không build trong Day 06**:

- API đặt lịch Vinmec thật + OTP production
- VinBigdata / chat production
- Lưu PII vào vector DB / fine-tune từ transcript
- Đa ngôn ngữ, bảo hiểm, thanh toán

---

## Kết quả nhóm Team Corner — Vinmec (đã chốt — cập nhật)

### Cụm evidence

- Symptom không map được khoa/BS trong chat thật
- Handoff link — user điền lại form từ đầu
- Form Vinmec tách field đặt lịch vs thông tin khách hàng → mô hình 2 phase phù hợp

### Insight

```text
User cần agent đặt lịch từ triệu chứng, không chỉ link.
Họ cần form gần hoàn tất sau chat và không muốn đưa SĐT/họ tên vào LLM,
vì chat thật không nối symptom → booking và form thật đã tách PII.
```

### Opportunity

```text
AI agent trong chat: LLM + mock (cơ sở, khoa, bác sĩ, slot) từ triệu chứng + tuổi + khu vực.
Handoff bookingDraft → form pre-fill (không LLM).
User chỉ nhập PII trên form và submit — dữ liệu cá nhân không qua prompt.
```

### Build slice — checklist

| Câu hỏi | Đạt? |
|---|---|
| User cụ thể? | Có — lần đầu đặt khám, mô tả triệu chứng |
| Task hẹp? | Có — agent + pre-fill + PII form (mock, 1 ngày) |
| AI decision rõ? | Có — map symptom → 2–3 khoa; chọn BS/slot từ mock |
| Failure path? | Có — sai khoa, red flag, PII trong chat bị từ chối |
| Evidence? | Có screenshot Vinmec + form web |

### Quyết định scope

**Giữ** pain Vinmec · **Mở rộng có kiểm soát:** chat agent + form (vẫn mock) · **Augmentation + privacy:** LLM không nhận PII · **Submit** ngoài LLM.

### Kiến trúc 2 phase (tóm tắt)

| Phase | Có LLM? | Dữ liệu |
|-------|---------|---------|
| A — Chat agent | Có | Triệu chứng, tuổi, cơ sở; mock khoa/BS/slot |
| B — Form đặt lịch | **Không** | Pre-fill từ `bookingDraft`; user nhập PII; submit mock |

### Câu chốt cuối

```text
Dựa trên evidence VinmecCare + form đăng ký khám,
nhóm sẽ build chatbot AI agent (symptom → gợi ý khoa/BS từ mock data),
handoff sang form đã fill thông tin đặt lịch,
user chỉ điền thông tin cá nhân và submit không qua LLM,
và test red flag + gợi ý khoa sai + từ chối PII trong chat.
```
