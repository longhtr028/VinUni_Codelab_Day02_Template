# Báo Cáo Sản Phẩm Phase 3 & 5: Phân Tích Sâu Dự Án AI (Deep-Dive Report)
**Dự án:** Trợ lý AI Đồng hành Điều phối Thông minh Xanh SM (Xử lý sự cố pin EV thực địa)  
**Đơn vị:** Khối Vận Hành Xanh SM (GSM) / Vin Smart Future — Tập đoàn Vingroup  
**Phiên bản tài liệu:** 1.0 (Quy chuẩn Scoping Sản phẩm)  

---

## 🏗️ 1. Sơ Đồ Quy Trình Hiện Tại (Current-State Workflow Mapping)

Quy trình xử lý thủ công hiện tại khi tài xế taxi điện Xanh SM gặp sự cố pin khẩn cấp:

```text
┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐
│ Bước 1          │       │ Bước 2          │       │ Bước 3 (🔴)      │       │ Bước 4 (🔴)      │
│ Tiếp nhận cuộc  │ ────> │ Tra cứu định vị │ ────> │ Tìm & khớp trạm │ ────> │ Soạn văn bản    │
│ gọi sự cố       │       │ GPS & mức pin   │       │ sạc còn trụ     │       │ hướng dẫn gửi TX│
│                 │       │                 │       │                 │       │                 │
│ Ai: Điều phối   │       │ Ai: Điều phối   │       │ Ai: Điều phối   │       │ Ai: Điều phối   │
│ ⏱ 2 phút        │       │ ⏱ 2 phút        │       │ ⏱ 5 phút        │       │ ⏱ 5 phút        │
│ In: Cuộc gọi    │       │ In: Biển số xe  │       │ In: GPS, Chuẩn  │       │ In: Dữ liệu thô │
│ Out: Phiếu lỗi  │       │ Out: Toạ độ xe  │       │ Out: Trạm sạc   │       │ Out: SMS gửi TX │
└─────────────────┘       └─────────────────┘       └─────────────────┘       └─────────────────┘
                                                                                      │
                                                                                      ▼
                                                                            ┌─────────────────┐
                                                                            │ Bước 5 (🔄)      │
                                                                            │ Điều phối xe cứu│
                                                                            │ hộ sạc di động  │
                                                                            │                 │
                                                                            │ Ai: Đội cứu hộ  │
                                                                            │ ⏱ 1 phút        │
                                                                            │ ĐK: Pin < 5%    │
                                                                            └─────────────────┘

⏱ Tổng thời gian xử lý thủ công: ~15 phút / lượt sự cố.
🔴 = Điểm nghẽn cổ chai (Tra cứu phân tán giữa các Dashboard và soạn tin thủ công).
🔄 = Điểm chuyển giao (Bàn giao ca trực tiếp giữa Điều phối trung tâm và Đội xe cứu hộ thực địa).
```

---

## 📋 2. Báo Cáo Vấn Đề (Problem Statement Chuẩn 6 Trường)

| Trường thông tin | Quy cách vận hành chi tiết |
|---|---|
| **1. Actor / Operator** | Điều phối viên (Dispatcher) thuộc Trung tâm Điều vận Xanh SM (Hà Nội & TP.HCM). |
| **2. Current Workflow** | Khi tài xế báo cạn pin dưới 10%, điều phối viên mở bản đồ GPS nội bộ, mở tiếp Dashboard mạng lưới trạm sạc VinFast tìm trụ sạc tương thích còn trống (CCS2/GBT), tự tính cự ly, gõ tin nhắn hướng dẫn đường đi và mẹo tiết kiệm điện gửi qua App tài xế, và gọi xe cứu hộ nếu pin dưới 5%. Quy trình 5 bước thủ công, mất 15 phút. |
| **3. Bottleneck** | **Bước 3 & 4 (chiếm 10/15 phút):** Phải đối chiếu thủ công thông số cổng sạc xe (VF5, VF8, VF9), trạng thái thực tế của trụ sạc, và tự gõ văn bản tiếng Việt chi tiết dẫn đường. |
| **4. Business Impact** | Mỗi ngày có ~80 sự cố pin tại Hà Nội/TP.HCM, gây thất thoát ~20 giờ làm việc của nhân viên điều vận. Xe nằm chờ gây lãng phí ~15% doanh thu cuốc xe, tốn chi phí kéo xe cứu hộ (~500.000 VNĐ/lần chết máy) và tăng tỉ lệ khách hủy chuyến do thiếu xe. |
| **5. Success Metric** | **1. Tốc độ:** Giảm thời gian xử lý sự cố từ 15 phút xuống **dưới 3 phút**.<br>**2. Độ chính xác:** Tỉ lệ chỉ đúng trạm có trụ tương thích đạt **trên 98%**.<br>**3. An toàn:** **0 trường hợp xe cạn pin chết máy** giữa đường do AI gợi ý trạm sạc vượt quá tầm di chuyển an toàn của pin. |
| **6. Operational Boundary** | **AI ĐƯỢC PHÉP:** Đọc telemetry xe (GPS, % pin), truy vấn API kiểm tra trạm sạc VinFast còn trống, và soạn sẵn tin nhắn hướng dẫn dạng nháp (Draft).<br>**RANH GIỚI CẤM (TUYỆT ĐỐI):**<br>- CẤM AI tự ý gửi tin nhắn trực tiếp đến tài xế khi chưa có điều phối viên bấm duyệt (bắt buộc luôn gắn thẻ `[DRAFT_ONLY]` ở đầu tin).<br>- Nếu pin **dưới 5%**, CẤM AI đề xuất bất kỳ trạm sạc nào cách xa quá 5km; AI phải lập tức xuất payload JSON gọi xe cứu hộ: `{"action": "dispatch_mobile_charger", "reason": "..."}`. |

---

## ⚡ 3. Ma Trận AI Fit & Quy Trình Tương Lai (Future-State Flow)

### Đánh giá mức độ phù hợp AI (AI-Fit Matrix)
* **Rule-based (Dựa trên luật):** Xử lý tốt việc tính khoảng cách toạ độ (`haversine`) và đối khớp loại cổng cắm, nhưng không thể diễn giải các tin nhắn phản hồi tự do từ tài xế cũng như không thể tạo tin nhắn hướng dẫn đường đi giàu ngữ cảnh, linh hoạt theo tình huống khẩn cấp.
* **LLM Feature (Trợ lý đồng hành Co-pilot) [ĐƯỢC CHỌN]:** Lựa chọn tối ưu nhất. Một lệnh gọi LLM kiểm soát ranh giới nghiêm ngặt sẽ nhận dữ liệu định vị, tự động so khớp, kiểm tra ranh giới pin và tạo tin nhắn nháp kèm lệnh cứu hộ có cấu trúc cho con người duyệt chỉ với 1 click.
* **Autonomous Multi-Agent Loop (Đa tác tử tự trị):** Bị từ chối. Lĩnh vực an toàn xe điện và điều phối giao thông thực địa có rủi ro pháp lý cao; việc dùng nhiều agent tự trị chạy lặp sẽ làm tăng độ trễ (latency) và nguy cơ ảo giác (hallucination) mà không đem lại giá trị vượt trội.

### Quy Trình Tương Lai Tích Hợp AI (Human-In-The-Loop)

```text
┌──────────────────┐       ┌────────────────────────┐       ┌────────────────────────┐       ┌──────────────────────┐
│ Tài xế báo sự cố │       │ 🔵 Hệ thống tự động lấy│       │ 🔵 AI Co-Pilot phân tích│       │ 🟢 Điều phối viên     │
│ khẩn cấp về pin  │ ────> │ GPS xe & dữ liệu trạm  │ ────> │ Soạn nháp & tạo lệnh   │ ────> │ Xem nhanh & 1-Click   │
│                  │       │ sạc trống từ Cloud     │       │ cứu hộ theo ranh giới  │       │ Phê duyệt gửi tài xế │
│ ⏱ Tức thời       │       │ ⏱ < 3 giây             │       │ ⏱ < 2 giây             │       │ ⏱ < 30 giây          │
└──────────────────┘       └────────────────────────┘       └────────────────────────┘       └──────────────────────┘
                                                                                                        │
                                                                    ┌───────────────────────────────────┴───────────────────────────────────┐
                                                                    ▼                                                                       ▼
                                                       [Trường hợp bình thường: Pin >= 5%]                     [Trường hợp khẩn cấp: Pin < 5%]
                                                       Tin nhắn [DRAFT_ONLY] sau khi duyệt                     Lệnh JSON điều xe sạc cứu hộ di động
                                                       sẽ tự động gửi vào App tài xế.                          được kích hoạt ngay lập tức.
                                                                    │
                                                                    ▼
                                                       ↩️ Cơ chế dự phòng (Fallback):
                                                       Nếu AI gặp sự cố mạng hoặc vượt quá 5 giây,
                                                       màn hình điều phối tự chuyển về bản đồ trạm sạc
                                                       truyền thống để con người thao tác tay như cũ.
```

---

## 🏁 4. Phase 5 — Đánh Giá Độ Sẵn Sàng (AI Readiness) & Quyết Định Cuối Cùng

### Checklist Đánh giá độ sẵn sàng
1. **Dữ liệu & Tích hợp:** **ĐẠT.** Hệ thống quản lý đội xe GSM và mạng lưới trạm sạc VinFast đã có sẵn API truy xuất thời gian thực (định vị GPS, % pin SoC, cổng sạc, trạng thái trụ trống).
2. **Kiểm soát rủi ro & An toàn:** **ĐẠT.** Thiết lập cơ chế kiểm duyệt người trong vòng lặp (**Human-In-The-Loop**) với thẻ bắt buộc `[DRAFT_ONLY]` và kiểm tra assertion tự động bằng code Python; loại bỏ 100% rủi ro AI tự ý phát ngôn sai.
3. **Sự sẵn sàng của nhân sự vận hành:** **ĐẠT.** Điều phối viên tại trung tâm Hà Nội/TP.HCM đang rất quá tải; việc chuyển từ gõ tay 15 phút sang duyệt 1 chạm dưới 30 giây giúp giảm áp lực công việc ngay lập tức.

### Quyết định của Ban Giám Đốc Vin Smart Future: **GO (Phê Duyệt Triển Khai)**

**Lý giải quyết định (Dựa trên kỹ thuật và hiệu quả kinh tế):**
- **Tính khả thi kỹ thuật:** Bản mẫu prompt nguyên mẫu chạy trên **Gemini Flash** trong file `prompt_prototype.py` đã chứng minh mô hình hoàn toàn tuân thủ ranh giới an toàn (xử lý chính xác ngưỡng pin 2% và duy trì thẻ duyệt nháp kể cả khi bị prompt tấn công).
- **Lợi ích kinh tế:** Giảm thời gian xử lý sự cố từ 15 phút xuống < 3 phút giúp tiết kiệm 16 giờ làm việc của nhân sự mỗi ngày, giảm thiểu hư hại pin cạn kiệt, ước tính tiết kiệm cho Xanh SM hơn 350 triệu VNĐ mỗi năm chi phí vận hành và xe cứu hộ kéo đường.
- **Rủi ro kiểm soát triệt để:** Nhờ cơ chế **HITL** và phương án dự phòng **Fallback** trực tiếp trên Dashboard, dự án không gây bất kỳ gián đoạn nào đến quy trình vận hành sẵn có của tập đoàn.
