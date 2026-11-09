# Báo Cáo Sản Phẩm Phase 1 & 2: Quét Cơ Hội & Thẻ Bài Toán Nhanh
**Đơn vị:** Vin Smart Future — Tập đoàn Vingroup  
**Người thực hiện / Đóng góp:** Kỹ sư AI Product (AI Product Engineer)  

---

## 🏛️ Bối cảnh dự án
Đóng vai trò là **Kỹ sư AI Product** tại **Vin Smart Future**, nhiệm vụ của chúng tôi là khảo sát các điểm nghẽn vận hành thực tế tại các công ty thành viên Vingroup (*VinFast, Xanh SM, Vinhomes, Vinmec, Vinpearl*) nhằm định vị các bài toán AI mang lại giá trị cao, khả thi về mặt kỹ thuật và thiết lập ranh giới vận hành tuyệt đối an toàn.

---

# 🔍 Phase 1 — SCAN: Bảng Quét Cơ Hội Vận Hành

Sử dụng **4 Thấu kính tìm bài toán AI** (*Lặp lại, Tốn thời gian, AI-upgrade, Pain từ người khác*), chúng tôi đã khảo sát và ghi nhận 6 bài toán vận hành tiêu biểu:

| # | Công ty thành viên | Tên bài toán / Điểm nghẽn vận hành | Thấu kính (Lens) | Bằng chứng & Nỗi đau thực tế |
|---|--------------------|-----------------------------------|------------------|------------------------------|
| 1 | **Xanh SM (GSM)** | Điều phối và xử lý khẩn cấp sự cố hết pin thực địa | Tốn thời gian | Tài xế cạn pin (<10%) gọi về trung tâm; điều phối viên phải tra cứu thủ công định vị GPS, kiểm tra trụ trống trên Dashboard VinFast và gõ tin nhắn chỉ đường (mất 12–15 phút/lượt). |
| 2 | **Vinhomes** | Phân loại và điều hướng phản ánh, khiếu nại của cư dân | Lặp lại | Bộ phận CSKH phải đọc và phân loại thủ công hơn 1.200 yêu cầu mỗi ngày (rò rỉ nước, ồn ào thang máy, thẻ xe) gửi qua App Vinhomes Resident; phản hồi mất tới 12 tiếng. |
| 3 | **VinFast** | Đối soát hóa đơn sạc điện từ các trạm sạc đối tác | Lặp lại | Đội ngũ tài chính phải kiểm tra thủ công dữ liệu tiêu thụ điện hàng tuần từ hơn 3.000 điểm sạc liên kết ngoài so với số liệu viễn thám thực tế của xe. |
| 4 | **Vinmec** | Soạn thảo tóm tắt hồ sơ bệnh án xuất viện (Discharge Summary) | Tốn thời gian | Bác sĩ mất từ 20–30 phút/bệnh nhân để tổng hợp lịch sử xét nghiệm, biên bản phẫu thuật và đơn thuốc từ bệnh án điện tử thành bản tóm tắt xuất viện dễ hiểu cho người bệnh. |
| 5 | **Vinpearl** | Quét và cảnh báo đánh giá tiêu cực khẩn cấp của khách hàng | Pain từ người khác | Quản lý tiền sảnh phải rà soát thủ công hàng trăm review mỗi ngày trên Agoda, Booking.com, Google Maps để kịp thời phát hiện các phàn nàn nghiêm trọng về vệ sinh hay thái độ phục vụ. |
| 6 | **VinFast** | Phân loại mã lỗi kỹ thuật từ mô tả âm thanh bất thường | AI-upgrade | Khách hàng mô tả lỗi bằng ngôn ngữ tự nhiên (ví dụ: *"xe phát ra tiếng kêu lục cục dưới gầm khi giảm tốc"*), kỹ thuật viên xưởng dịch vụ mất nhiều thời gian hỏi lại để định vị sơ bộ lỗi. |

---

# 🃏 Phase 2 — QUICK-ASSESS: 3 Thẻ Bài Toán Nhanh (Quick Cards)

Từ bảng quét trên, nhóm chắt lọc ra 3 bài toán tiềm năng nhất để phân tích chi tiết:

### Thẻ Bài Toán Nhanh #1 (Được chọn để Deep-Dive)
```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                                       │
│                                                                             │
│ Bài toán: Tài xế Xanh SM báo sự cố cạn kiệt pin giữa đường cần điều phối    │
│ trạm sạc tương thích gần nhất hoặc điều xe sạc cứu hộ di động khẩn cấp.     │
│ Công ty thành viên: [x] Xanh SM (GSM)                                       │
│                                                                             │
│ Ai đang đau (Actor)? Điều phối viên trung tâm vận hành & Tài xế taxi Xanh. │
│                                                                             │
│ Workflow thủ công hiện tại (5 bước):                                        │
│   1. Tài xế gọi hotline khẩn cấp báo pin yếu / sắp tắt máy                  │
│   → 2. Điều phối viên tra cứu vị trí GPS và dung lượng pin trên bản đồ xe   │
│   → 3. Mở Dashboard trạm sạc VinFast tìm trụ trống đúng chuẩn cổng (CCS2)   │
│   → 4. Tự gõ tin nhắn SMS chỉ dẫn đường và mẹo tiết kiệm pin gửi tài xế     │
│   → 5. Liên hệ đội xe sạc cứu hộ lưu động nếu pin dưới 5%                   │
│                                                                             │
│ Bước tốn thời gian/dễ lỗi nhất: Bước 3 & 4 (⏱ 10–12 phút/lượt)              │
│ AI có thể nhảy vào hỗ trợ ở bước nào: Bước 3 & 4 (Đọc dữ liệu xe -> Tự động  │
│ tìm trạm trống tối ưu -> Soạn nháp SMS có ranh giới an toàn).               │
│                                                                             │
│ Đo thành công bằng gì (Metric có số)?                                        │
│   - Giảm thời gian xử lý sự cố từ 15 phút xuống dưới 3 phút.                │
│   - Tỉ lệ xe bị chết máy giữa đường do chỉ nhầm trạm sạc: 0%.              │
│                                                                             │
│ Quick Architecture: [x] LLM Feature (Co-pilot với bước duyệt nháp bắt buộc)  │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### Thẻ Bài Toán Nhanh #2
```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                                       │
│                                                                             │
│ Bài toán: Xử lý và phân loại chậm trễ các phiếu phản ánh dịch vụ của cư dân │
│ gửi qua ứng dụng di động Vinhomes Resident.                                 │
│ Công ty thành viên: [x] Vinhomes                                            │
│                                                                             │
│ Ai đang đau (Actor)? Nhân viên CSKH & Ban Quản lý các tòa nhà Vinhomes.     │
│                                                                             │
│ Workflow thủ công hiện tại (4 bước):                                        │
│   1. Cư dân gửi phản ánh (văn bản kèm hình ảnh) trên ứng dụng               │
│   → 2. Nhân viên CSKH đọc nội dung và gắn nhãn phân loại (Điện/Nước/An ninh)│
│   → 3. Kiểm tra ca trực kỹ thuật và chuyển tiếp phiếu đến tổ phụ trách      │
│   → 4. Gõ tin nhắn xác nhận tiếp nhận gửi lại cho cư dân                    │
│                                                                             │
│ Bước tốn thời gian/dễ lỗi nhất: Bước 2 & 3 (⏱ 8 phút/phiếu; gây dồn ứ)      │
│ AI có thể nhảy vào hỗ trợ ở bước nào: Bước 2 (Phân loại ý định và ưu tiên)  │
│                                                                             │
│ Đo thành công bằng gì (Metric có số)?                                        │
│   - Thời gian phản hồi lần đầu (SLA) giảm từ 12 giờ xuống dưới 15 phút.     │
│   - Độ chính xác điều hướng đúng phòng ban kỹ thuật đạt trên 95%.           │
│                                                                             │
│ Quick Architecture: [x] Bộ phân loại dựa trên luật (Rule) + Router LLM      │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

### Thẻ Bài Toán Nhanh #3
```text
┌─────────────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                                       │
│                                                                             │
│ Bài toán: Bác sĩ Vinmec mất quá nhiều thời gian tổng hợp hồ sơ bệnh án      │
│ phức tạp thành tài liệu tóm tắt xuất viện dễ hiểu cho bệnh nhân.            │
│ Công ty thành viên: [x] Vinmec                                              │
│                                                                             │
│ Ai đang đau (Actor)? Bác sĩ điều trị & Điều dưỡng khoa nội trú.             │
│                                                                             │
│ Workflow thủ công hiện tại (4 bước):                                        │
│   1. Bác sĩ rà soát toàn bộ kết quả xét nghiệm, chẩn đoán hình ảnh nhiều ngày│
│   → 2. Gõ bản tóm tắt diễn tiến điều trị vào hệ thống bệnh án điện tử HIS   │
│   → 3. Viết lại các thuật ngữ y khoa thành hướng dẫn chăm sóc tại nhà       │
│   → 4. Điều dưỡng in tài liệu và dặn dò lịch uống thuốc cho bệnh nhân        │
│                                                                             │
│ Bước tốn thời gian/dễ lỗi nhất: Bước 2 & 3 (⏱ 25 phút/bệnh nhân)            │
│ AI có thể nhảy vào hỗ trợ ở bước nào: Bước 3 (Tự động soạn thảo bản tóm tắt │
│ ngôn ngữ đại chúng từ dữ liệu hồ sơ lâm sàng có cấu trúc).                  │
│                                                                             │
│ Đo thành công bằng gì (Metric có số)?                                        │
│   - Giảm thời gian soạn bản tóm tắt từ 25 phút xuống dưới 5 phút.           │
│   - Không có sai sót y khoa (Bắt buộc 100% bác sĩ ký duyệt trước khi in).   │
│                                                                             │
│ Quick Architecture: [x] LLM Feature có con người kiểm duyệt bắt buộc        │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 🗳️ Quyết Định Lựa Chọn Của Nhóm & Lý Do Loại Trừ

Nhóm đã thống nhất 100% lựa chọn **Thẻ #1 (Trợ lý AI Điều phối Khẩn cấp Pin Xanh SM)** để tiến hành Phân tích sâu (Deep-Dive):

1. **Tính cấp thiết & Tác động vận hành trực tiếp:** Khác với các tác vụ văn phòng xử lý offline, sự cố xe điện chết máy giữa đường ảnh hưởng trực tiếp đến thu nhập tài xế, ách tắc giao thông công cộng, an toàn hành khách và hao mòn pin xe.
2. **Ranh giới vận hành rõ ràng, đo lường được:** Tính chất rủi ro cao giúp việc thiết lập ranh giới an toàn rất chặt chẽ và khả thi (ví dụ: cấm đề xuất trạm xa khi pin dưới 5%, bắt buộc giữ thẻ `[DRAFT_ONLY]` để điều phối viên duyệt trước khi gửi).
3. **Lý do tạm hoãn Thẻ #2 và Thẻ #3:**
   - *Thẻ #2 (Vinhomes):* Bản chất là bài toán định tuyến (Routing). Một hệ thống phân loại theo Rule-based kết hợp Regex/NLP truyền thống có thể giải quyết tốt, không cần tốn chi phí gọi LLM lớn.
   - *Thẻ #3 (Vinmec):* Lĩnh vực y tế có rào cản pháp lý và an toàn bệnh nhân cực kỳ nghiêm ngặt, đòi hỏi quy trình thẩm định phức tạp vượt quá thời lượng của bài lab hiện tại.
