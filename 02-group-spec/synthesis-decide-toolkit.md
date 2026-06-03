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

- Tích hợp API đặt lịch Vinmec thật + OTP
- Chọn bác sĩ + slot giờ trong chat
- Đa ngôn ngữ / đặt hẹn người nước ngoài
- Học correction production VinBigdata

---

## Kết quả nhóm Team Corner — Vinmec (đã chốt)

### Cụm evidence

- Không biết chọn chuyên khoa sau khi mô tả triệu chứng
- Handoff chat → link web mất context
- Trả lời cơ sở/địa điểm mơ hồ không được làm rõ

### Insight

```text
User đặt khám lần đầu qua chat VinmecCare không chỉ cần link đặt lịch.
Họ cần hỗ trợ quyết định chuyên khoa an toàn và ít ma sát,
vì self-use cho thấy bot không map symptom → specialty trước khi chuyển form web.
```

### Opportunity

```text
Cơ hội là dùng AI để augment: hỏi làm rõ + gợi ý 2–3 chuyên khoa có lý do,
giúp user chọn khoa trước khi sang form/link,
trong khi vẫn kiểm soát red flag và gợi ý sai bằng low-confidence + hotline.
```

### Build slice — checklist

| Câu hỏi | Đạt? |
|---|---|
| User cụ thể? | Có — người lần đầu, mô tả triệu chứng tiếng Việt |
| Task hẹp? | Có — symptom → gợi ý khoa + confirm (không OTP/API) |
| AI decision rõ? | Có — rank/gợi ý chuyên khoa + confidence |
| Failure path? | Có — sai khoa + red flag |
| Evidence? | Có self-use + form web; review bổ sung sáng Day 06 |

### Quyết định scope

**Giữ** domain Vinmec · **Giảm scope** bỏ đặt lịch thật/API · **Augmentation** vì rủi ro y tế.

### Câu chốt cuối

```text
Dựa trên screenshot chat + form đăng ký khám Vinmec,
nhóm sẽ build prototype chat "symptom → gợi ý 2–3 chuyên khoa → xác nhận → summary",
cho người bệnh lần đầu đặt khám qua chat,
để giải quyết pain không biết chọn khoa sau handoff link,
bằng cách AI augment gợi ý chuyên khoa,
và sẽ test failure path gợi ý khoa sai + red flag đau bụng nặng.
```
