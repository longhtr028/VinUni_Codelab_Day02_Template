# 01 — Problem Scan & Quick Cards (Lab 02: AI Product Scoping)

- **Tên nhóm:** [ĐIỀN TÊN NHÓM]
- **Thành viên:**
  1. Phạm Nguyên Khoa — [ĐIỀN EMAIL] *(tác giả file này, branch `PhamNguyenKhoa`)*
  2. [Họ tên thành viên 2] — [email]
  3. [Họ tên thành viên 3] — [email]
  4. [Họ tên thành viên 4] — [email]

> **Lưu ý về số liệu:** mọi con số về thời gian, khối lượng, tỉ lệ trong file này là **giả định ước tính** của tôi (chưa có dữ liệu thật từ Vingroup) và được đánh dấu `(*)`. Khi làm sâu cùng nhóm, các số `(*)` cần được kiểm chứng hoặc thay bằng số đo thực tế.

---

# 🔍 Phase 1 — SCAN (Cá nhân)

Quét qua vận hành các công ty thành viên Vingroup bằng 4 lens: **Lặp lại**, **Tốn thời gian**, **AI có thể tốt hơn**, **Pain từ người khác**. Đây là 5 bài toán tôi quét được (tham khảo thêm gợi ý trong `03-inspiration-kit.md`).

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Xanh SM** | Tốn thời gian | Điều vận thông minh: tối ưu điểm đón khách dựa trên phân tích tin nhắn tiếng Việt của tài xế và tọa độ GPS thực tế, thay vì điều phối viên tự đọc và xử lý. |
| 2 | **VinFast** | AI có thể tốt hơn | Trợ lý hướng dẫn trạm sạc: tự động gợi ý lịch sạc và trạm còn trụ trống, phù hợp loại cổng sạc (CCS2/GB-T) của từng dòng xe (VF5, VF8, VF9). Hiện chủ xe phải tự dò bản đồ, dễ đi nhầm trạm. |
| 3 | **Vinmec** | Tốn thời gian | Soạn tóm tắt xuất viện: bác sĩ trích thông tin từ bệnh án điện tử, xét nghiệm, ghi chú rồi viết bản tóm tắt dễ hiểu cho bệnh nhân; ~20-30 phút/bệnh nhân `(*)`. |
| 4 | **Vinhomes** | Lặp lại | Phân loại và điều hướng phản ánh của cư dân (mất nước, hỏng đèn, ồn ào...) gửi qua app Vinhomes Resident đến đúng ban quản lý tòa nhà; hiện làm thủ công, lặp lại hàng trăm lượt/ngày `(*)`. |
| 5 | **Vinpearl** | Pain từ người khác | Tổng hợp và phân tích review khách sạn trên Booking.com, Agoda, Google Maps để lọc phàn nàn khẩn cấp ("phòng bẩn", "nhân viên thái độ kém") gửi cho quản lý. |

---

# 🃏 Phase 2 — QUICK-ASSESS (Cá nhân)

Từ 5 bài trên, tôi chọn top 3: **#2 (VinFast), #4 (Vinhomes), #5 (Vinpearl)**. Số card trùng với số bài trong bảng Scan. Việc cả nhóm chọn bài để Deep-Dive diễn ra ở Phase 3.

**Vì sao các bài này hợp lý (ý kiến cá nhân tôi):** có người dùng rõ ràng, bước nghẽn cụ thể, đo được bằng số, và rủi ro khi AI sai nằm trong tầm kiểm soát nhờ người duyệt (HITL) hoặc luật cứng.
**Vì sao #3 (Vinmec) kém phù hợp hơn:** dữ liệu y tế nhạy cảm, AI sai có thể ảnh hưởng đến bệnh nhân nên cần ranh giới và quy trình duyệt cực chặt, chưa hợp để làm bản mẫu đầu tiên. **#1 (Xanh SM)** cần dữ liệu GPS và tin nhắn tài xế theo thời gian thực, khó kiểm thử hơn trong phạm vi lab.

## Card #2 — VinFast: trợ lý hướng dẫn trạm sạc

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán (1 câu): Chủ xe VinFast khó biết trạm sạc nào còn  │
│ trụ trống và đúng loại cổng sạc, nên hay đi nhầm hoặc phải  │
│ gọi hotline hỏi.                                            │
│ Công ty thành viên: [x] VinFast                             │
│                                                             │
│ Ai đang đau (Actor)? Chủ xe điện VinFast (VF5/VF8/VF9) và   │
│ nhân viên hotline/CSKH phải trả lời cùng một loại câu hỏi.  │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│  1. Chủ xe thấy pin thấp, mở app/bản đồ tìm trạm sạc        │
│  ──> 2. Tự dò danh sách trạm, không rõ trụ trống/loại cổng  │
│  ──> 3. Lái đến trạm, mới phát hiện hết trụ hoặc không hợp  │
│  ──> 4. Gọi hotline hỏi trạm khác                           │
│  ──> 5. CSKH tra thủ công và hướng dẫn lại                  │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-3                   │
│   (⏱ mất thêm ~15-30 phút/lượt đi nhầm) (*)                 │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2: hiểu câu hỏi  │
│   tự nhiên ("pin còn 20%, đi Hà Nội - Hải Phòng"), lọc trạm │
│   đúng cổng và còn trụ, soạn gợi ý lịch sạc.                │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   - Khuyến nghị đúng (trụ tương thích và còn trống khi đến) │
│     >= 95% (*)                                              │
│   - Giảm 30% cuộc gọi hotline về tìm trạm sạc (*)           │
│   - 0 trường hợp gợi ý trạm không tương thích cổng sạc      │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent │
│   (Rule: lọc cổng sạc, biên an toàn pin/quãng đường;        │
│    LLM: hiểu yêu cầu tiếng Việt và soạn gợi ý)              │
└─────────────────────────────────────────────────────────────┘
```

**Ranh giới đề xuất:** AI chỉ **gợi ý**, không tự đặt chỗ hay thanh toán. Chỉ dùng dữ liệu trạm đang cập nhật; nếu dữ liệu cũ phải nói rõ. Khi pin dưới 5%, không gợi ý trạm xa hơn 5km mà đề xuất xe sạc di động (giống luật trong prototype của lab).

**Tự phản biện (đóng vai CFO/Trưởng vận hành):**
1. *Lọc trạm theo cổng sạc và khoảng cách là truy vấn dữ liệu và rule, không cần LLM.* → LLM chỉ nên dùng để hiểu câu hỏi tự do và soạn lời hướng dẫn; phần chọn trạm giao cho code thường.
2. *Gợi ý đúng hay không phụ thuộc vào API trụ trống có realtime hay không.* → Cần xác nhận độ tươi của dữ liệu trước khi cam kết metric 95%.
3. *Ước tính quãng đường theo pin sai có thể khiến xe hết pin giữa đường.* → Luôn có biên an toàn và luật cứng cho ngưỡng pin thấp.

## Card #4 — Vinhomes: phân loại và điều hướng phản ánh của cư dân

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #4                                       │
│                                                             │
│ Bài toán (1 câu): Phản ánh của cư dân trên app được nhân    │
│ viên đọc và chuyển thủ công đến bộ phận xử lý, chậm và dễ   │
│ chuyển nhầm.                                                │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau (Actor)? Nhân viên CSKH/ban quản lý tòa nhà,    │
│ cư dân (chờ phản hồi lâu).                                  │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│  1. Cư dân gửi phản ánh trên app                            │
│  ──> 2. Nhân viên đọc và phân loại (kỹ thuật/an ninh/vệ sinh)│
│  ──> 3. Chuyển thủ công đến bộ phận phụ trách               │
│  ──> 4. Bộ phận xử lý                                       │
│  ──> 5. Nhân viên phản hồi lại cư dân                       │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2 và 3                │
│   (⏱ phản hồi đầu tiên có thể mất vài giờ) (*)              │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-3: gợi ý nhãn  │
│   phân loại, mức độ khẩn, bộ phận nhận.                     │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   - Thời gian phân loại ──> dưới 1 phút/phản ánh (*)        │
│   - Độ chính xác phân loại >= 90% trên 200 phản ánh gán     │
│     nhãn tay (*)                                            │
│   - 100% phản ánh khẩn (cháy, rò rỉ gas, mất nước diện      │
│     rộng) được gắn cờ cho người xử lý ngay                  │
│                                                             │
│ Quick Architecture: [ ] No AI  [x] Rule  [x] LLM  [ ] Agent │
│   (Rule: từ khoá an toàn luôn đẩy lên khẩn; LLM: phân loại  │
│    các phản ánh viết tự do)                                 │
└─────────────────────────────────────────────────────────────┘
```

**Tự phản biện:**
1. *Nếu phản ánh chỉ có vài nhãn cố định, form chọn danh mục trên app (rule-based) có thể giải quyết rẻ hơn LLM.* → Cần xem tỉ lệ phản ánh viết tự do, không khớp danh mục.
2. *Phân loại sai một phản ánh khẩn là rủi ro lớn.* → Bắt buộc người duyệt các nhãn "khẩn" và có fallback: nếu model không chắc thì chuyển cho người.
3. *Chưa có bộ dữ liệu gán nhãn.* → Việc đầu tiên là gán nhãn khoảng 200 phản ánh để làm baseline.

## Card #5 — Vinpearl: phát hiện phàn nàn khẩn trong review khách sạn

```
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #5                                       │
│                                                             │
│ Bài toán (1 câu): Quản lý khách sạn phải đọc tay review trên│
│ nhiều kênh để tìm phàn nàn khẩn cần xử lý.                  │
│ Công ty thành viên: [x] Khác: Vinpearl                      │
│                                                             │
│ Ai đang đau (Actor)? Quản lý khách sạn/Guest Relations;     │
│ khách lưu trú (phàn nàn không được phản hồi kịp).           │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│  1. Mở từng kênh review (Booking, Agoda, Google Maps)       │
│  ──> 2. Đọc từng review mới                                 │
│  ──> 3. Ghi các phàn nàn vào bảng theo dõi                  │
│  ──> 4. Phân loại (vệ sinh, thái độ, cơ sở vật chất...)     │
│  ──> 5. Báo bộ phận liên quan xử lý                         │
│                                                             │
│ Bước nào tốn thời gian/lỗi nhất? Bước 2-4                   │
│   (⏱ ~2 giờ/ngày cho mỗi khách sạn) (*)                     │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2-4: tóm tắt,    │
│   gắn nhãn chủ đề và mức khẩn, soạn NHÁP phản hồi.          │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                       │
│   - Phát hiện phàn nàn nghiêm trọng trong dưới 1 giờ sau    │
│     khi review được đăng (hiện: vài giờ đến vài ngày) (*)   │
│   - Bắt được >= 90% review nghiêm trọng trên bộ mẫu gán     │
│     nhãn tay (*)                                            │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

**Tự phản biện:**
1. *Thời gian "2 giờ/ngày" là ước đoán.* → Cần hỏi quản lý khách sạn hoặc đo một tuần thực tế.
2. *AI soạn phản hồi công khai thay mặt thương hiệu có rủi ro hình ảnh.* → AI chỉ soạn nháp, người duyệt mới đăng; không bao giờ tự đăng.
3. *Review tiếng nước ngoài và mỉa mai có thể bị hiểu sai.* → Cần thử trên mẫu review đa ngôn ngữ trước khi tin vào số 90%.

---

## Tóm tắt so sánh 3 card

| Card | Công ty | Rủi ro khi AI sai | Dữ liệu sẵn có (giả định) | Độ phù hợp để làm bản mẫu |
|------|---------|-------------------|---------------------------|---------------------------|
| #2 | VinFast | Cao (xe hết pin giữa đường) → cần luật cứng + biên an toàn | Cần API trụ sạc realtime | Cao (khớp luật pin dưới 5% của prototype) |
| #4 | Vinhomes | Trung bình (phản ánh khẩn bị chậm) | Cần gán nhãn thêm | Trung bình |
| #5 | Vinpearl | Thấp-trung bình (phản hồi sai giọng) | Review công khai dễ thu thập | Cao |

*(Việc chọn một bài để Deep-Dive sẽ do cả nhóm thảo luận và quyết định.)*
