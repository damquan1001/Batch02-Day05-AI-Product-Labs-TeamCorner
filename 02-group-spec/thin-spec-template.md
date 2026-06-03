# Thin SPEC — VinmecCare · Symptom → Chuyên khoa

Thin SPEC không phải PRD đầy đủ. Đây là bản cam kết đủ rõ để sáng Day 06 nhóm build ngay.

## 1. Track, product/app và user

**Track:** Healthcare  
**Product/app thật:** Trợ lý ảo VinmecCare (Vinmec) + form [Đăng ký khám](http://vinmec.com/vie/dang-ky-kham/)  
**User cụ thể:** Người bệnh lần đầu (hoặc người nhà) đặt khám ngoại trú, mô tả triệu chứng bằng tiếng Việt tự nhiên, không rành phân khoa  
**Nhóm có phải user thật không? Nếu không, khác ở đâu?** Một phần là user (đã thử chat); khác ở mức độ bệnh nặng — prototype không thay tư vấn y khoa, chỉ hỗ trợ chọn khoa sơ bộ  

## 2. Evidence summary

| Evidence | Nguồn | User/pain nói lên điều gì? | SPEC phải đổi gì? |
|---|---|---|---|
| Chat: triệu chứng → tuổi → cơ sở → link | Screenshot self-use | Pain ở handoff, không chọn khoa trong chat | Build slice chỉ tập trung gợi ý khoa |
| "Vin ở Hà Nội" → link + hotline | Screenshot self-use | Địa điểm mơ hồ, low-confidence yếu | Thêm hỏi lại cơ sở (2–3 gợi ý HN) |
| Form bắt buộc chuyên khoa | vinmec.com/vie/dang-ky-kham/ | User phải tự quyết khoa trên web | Output = summary có khoa đã chọn |

## 3. Pain statement

```text
User đang đau bụng / táo bón / khó đi lại và muốn hẹn khám qua chat VinmecCare,
gặp khó ở bước sau khi bot chỉ gửi link web,
vì form đăng ký bắt buộc tự chọn chuyên khoa và bác sĩ trong khi chat không chuyển tiếp được triệu chứng đã nói,
dẫn tới bỏ giữa chừng hoặc chọn sai khoa.
Bằng chứng chính là screenshot chat (3 bước + link) và form đăng ký khám Vinmec.
```

## 4. Build slice

```text
Cho người bệnh lần đầu đang mô tả triệu chứng và muốn đặt lịch qua chat,
prototype sẽ dùng AI để hỏi tối đa 2 câu làm rõ + gợi ý 2–3 chuyên khoa Vinmec phù hợp (kèm lý do ngắn),
tạo ra "Bản tóm tắt đặt lịch" (triệu chứng, tuổi, cơ sở gợi ý, chuyên khoa user chọn),
và xử lý failure mode (gợi ý khoa sai / triệu chứng nặng / red flag) bằng low-confidence (hỏi lại, 2 nhánh khoa)
+ nút gọi hotline / "không chắc — liên hệ người".
```

## 5. Auto/Aug decision

Chọn một:

- [x] **Augmentation:** AI gợi ý/draft/phân loại, user quyết cuối.
- [ ] **Conditional automation:** AI tự làm trong case hẹp; case mơ hồ/rủi ro chuyển người.
- [ ] **Automation:** AI tự quyết và tự hành động.

**Lý do chọn:** Rủi ro y tế cao nếu AI tự chọn khoa và tự submit; OTP/API/form Vinmec ngoài scope 1 ngày.  
**Human role:** decider (chọn 1 trong 2–3 khoa) + rescuer (hotline khi red flag / không chắc)  

## 6. Four paths

| Path | Prototype phải thể hiện gì? |
|---|---|
| Happy | Triệu chứng rõ → 2–3 khoa + lý do → user chọn → summary → CTA "Tiếp tục đặt lịch" (mock link form đã biết khoa) |
| Low-confidence | AI confidence thấp → hỏi thêm 1–2 câu HOẶC hiện 2 nhánh khoa + "Tôi không chắc" |
| Failure | User chọn "Khoa không đúng" → chọn lại từ danh sách / mô tả thêm |
| Correction | Sau khi chọn khoa, user đổi khoa trên summary; hiển thị "Đã cập nhật lựa chọn của bạn" (mock log) |

## 7. Failure mode nguy hiểm nhất

```text
Nếu user mô tả đau bụng nặng, táo bón kéo dài, không đi lại được / dấu hiệu cấp cứu,
AI có thể gợi ý nhầm khoa ngoại trú thay vì hướng dẫn cấp cứu,
hậu quả là trễ điều trị.
Prototype sẽ xử lý bằng red-flag checklist → chặn gợi ý khoa thường,
hiện cảnh báo + hotline Vinmec (số từ bot thật, ví dụ Times City 02439743556).
Owner kiểm thử path này là Khoa (2A202600922).
```

## 8. Owner plan cho sáng Day 06

| Thành viên | Việc phụ trách | Bằng chứng cần có trong repo |
|---|---|---|
| Khoa (2A202600922) | Research / evidence + SPEC | evidence-pack + thin-spec + 2 review thật sáng Day 06 |
| [Thành viên 2] | Prototype UI chat | Demo flow happy path |
| [Thành viên 3] | Test / failure path | Script: happy / mơ hồ HN / red flag |
| [Thành viên 4] | Demo script / repo | README ngắn + recording 3 phút |
