 
# Báo cáo Deep-Dive — Tổng hợp phản ánh cư dân Vinhomes

> **Phạm vi:** Phân tích sâu Quick Problem Card #3 ở [01-problem-scan.md](01-problem-scan.md). Báo cáo này là đề xuất scoping, không khẳng định mô tả quy trình hay số liệu là dữ liệu nội bộ đã được Vinhomes xác nhận.

## Quyết định chọn bài toán

Chọn bài toán **tổng hợp phản ánh cư dân**: nhận diện chủ đề lặp lại hoặc nghiêm trọng từ phản ánh và khảo sát, tạo tóm tắt ưu tiên cho ban quản lý. Trong ba ý tưởng Phase 2, bài toán này phù hợp để thử LLM ở bước đọc và tóm tắt ngôn ngữ tự do; các quyết định ưu tiên và điều phối vẫn do nhân viên thực hiện.

## 3.1. Current-State Workflow

Quy trình giả định cần được xác minh qua phỏng vấn nhân viên chăm sóc cư dân và ban quản lý. Thời gian dưới đây là ước tính cho một phản ánh đơn lẻ; không bao gồm thời gian chờ xử lý của bộ phận nhận việc.

```text
┌─────────────────────┐    ┌─────────────────────┐    ┌─────────────────────┐    ┌─────────────────────┐
│ Bước 1              │    │ Bước 2              │    │ Bước 3              │    │ Bước 4              │
│ Tiếp nhận phản ánh  │    │ Đọc nội dung,       │    │ Xác định mức độ     │    │ Ghi nhận, chuyển    │
│ / khảo sát từ các   │ →  │ làm rõ thông tin    │ →  │ ưu tiên, tìm phản   │ →  │ bộ phận phụ trách   │
│ kênh                 │    │ và phân loại        │    │ ánh tương tự        │    │ và tổng hợp         │
├─────────────────────┤    ├─────────────────────┤    ├─────────────────────┤    ├─────────────────────┤
│ Ai: CSKH / BQL      │    │ Ai: CSKH            │    │ Ai: CSKH / BQL      │    │ Ai: CSKH / BQL      │
│ ⏱ 1 phút            │    │ ⏱ 3 phút 🔴         │    │ ⏱ 2 phút 🔴         │    │ ⏱ 2 phút            │
│ In: Nội dung, kênh  │    │ In: Phản ánh        │    │ In: Nội dung và     │    │ In: Nhãn, mức độ    │
│ Out: Hồ sơ tiếp nhận│    │ Out: Chủ đề sơ bộ   │    │ lịch sử liên quan   │    │ Out: Ticket / báo cáo│
└─────────────────────┘    └─────────────────────┘    └─────────────────────┘    └─────────────────────┘
                                  🔄 chuyển thông tin giữa CSKH và ban quản lý/bộ phận xử lý

🔴 = bottleneck giả định; cần đo bằng quan sát thực tế.
⏱ Tổng thời gian thao tác ước tính: 8 phút/phản ánh.
```

**Điểm nghẽn:** Đọc nội dung tự do, phân loại nhất quán, tìm các phản ánh trùng/chủ đề tương tự và tổng hợp để chuyển xử lý. Nếu phản ánh thiếu thông tin, cần liên hệ cư dân làm rõ; thời gian này chưa nằm trong ước tính 8 phút.

**Ước tính tác động để lập giả thuyết:** Nếu có 100 phản ánh/ngày và mỗi phản ánh thực sự cần 8 phút xử lý thủ công, công việc tương đương khoảng **13,3 giờ công/ngày**. Đây chỉ là phép tính minh họa (100 × 8 ÷ 60), không phải số lượng hay chi phí thực tế; cần thay bằng số liệu đo tại đơn vị thí điểm.

## 3.2. Problem Statement (6-field)

| Field | Nội dung |
|---|---|
| **1. Actor / Operator** | Nhân viên chăm sóc cư dân tiếp nhận và ghi nhận phản ánh; ban quản lý hoặc bộ phận chuyên môn xác nhận mức độ ưu tiên và xử lý. Cư dân là người gửi phản ánh và chịu ảnh hưởng bởi chất lượng, tốc độ phản hồi. |
| **2. Current Workflow** | Theo quy trình giả định, nhân viên tiếp nhận phản ánh từ các kênh, đọc và phân loại nội dung, xác định độ ưu tiên/tìm phản ánh tương tự, rồi ghi nhận và chuyển bộ phận phụ trách. Thời gian thao tác ước tính 8 phút/phản ánh, cần được đo và xác minh tại cơ sở. |
| **3. Bottleneck** | Đọc và diễn giải văn bản tự do, tìm chủ đề lặp lại và tóm tắt thông tin cho bộ phận nhận việc. Nội dung ngắn, thiếu ngữ cảnh, trùng lặp hoặc viết không rõ ràng làm tăng khả năng phân loại sai và phải xử lý lại. |
| **4. Business Impact** | Thời gian nhân viên dành cho đọc, phân loại và tổng hợp có thể làm giảm thời gian dành cho xử lý các trường hợp cần trao đổi trực tiếp. Tác động về SLA, tồn đọng, phân loại sai và mức hài lòng chưa có số liệu xác thực. Đo baseline gồm số phản ánh/ngày, phút xử lý, tỷ lệ chuyển sai, tỷ lệ xử lý lại và thời gian phản hồi. |
| **5. Success Metric** | Trong pilot, giảm trung vị thời gian phân loại và tóm tắt từ **8 phút xuống ≤2 phút/phản ánh**; đạt **macro-F1 ≥0,90** cho phân loại trên tập đánh giá do nhân viên gán nhãn và **recall ≥0,95** cho nhóm khẩn cấp; theo dõi tỷ lệ nhân viên phải sửa tóm tắt. Đây là ngưỡng thử nghiệm đề xuất, cần xác nhận sau khi có baseline và tập dữ liệu đại diện; nếu chưa đạt recall mục tiêu thì không tự động đề xuất mức ưu tiên. |
| **6. Operational Boundary** | Chỉ dùng dữ liệu được phép truy cập và tối thiểu hóa thông tin nhận dạng cư dân; tuân thủ chính sách bảo mật, lưu trữ và phân quyền. LLM được phép tạo bản tóm tắt, gợi ý chủ đề và gợi ý mức ưu tiên kèm trích dẫn/đoạn nguồn. **Không** tự gửi phản hồi, tự đóng ticket, tự cam kết thời hạn/chi phí, kết luận trách nhiệm hoặc tự điều phối sự cố. Nhân viên phải duyệt trước khi lưu/chuyển kết quả. Trường hợp có nguy cơ an toàn, y tế, an ninh, tranh chấp, nội dung đe dọa hoặc độ tin cậy thấp phải chuyển người phụ trách ngay; nếu thiếu dữ liệu hay dịch vụ lỗi thì dùng quy trình thủ công. Nội dung cư dân là dữ liệu đầu vào không đáng tin cậy, không được xem là chỉ dẫn để thay đổi quy tắc hệ thống. |

### Cách đo metric trong pilot

1. Lấy mẫu phản ánh đã xử lý và đo thời gian thao tác hiện tại; thống nhất định nghĩa bắt đầu/kết thúc thời gian.
2. Tạo tập đánh giá đã ẩn danh, gồm nhiều loại phản ánh, cách diễn đạt và trường hợp khẩn cấp; nhân viên gán nhãn chuẩn độc lập hoặc qua quy trình đồng thuận.
3. So sánh quy trình hiện tại với quy trình có AI trên cùng nhóm tác vụ; báo cáo trung vị và phân vị 90 của thời gian, độ chính xác theo từng lớp, tỷ lệ chuyển sai và tỷ lệ sửa kết quả.
4. Chỉ mở rộng nếu đạt ngưỡng recall đã thống nhất cho trường hợp khẩn cấp; đánh giá riêng các lỗi nghiêm trọng, không chỉ dựa vào chỉ số tổng thể.

## 3.3. Future-State Flow & AI Fit

**AI Fit đề xuất: LLM Feature có kiểm soát, không dùng Agentic Loop.** LLM có lợi thế ở việc tóm tắt và gợi ý nhãn cho ngôn ngữ tự do, nhưng không nên tự thực hiện hành động vận hành. Các trường có cấu trúc (kênh, tòa nhà, mã ticket, trạng thái) nên được xử lý bằng validation và quy tắc thông thường.

| Phương án | Đánh giá |
|---|---|
| **Rule / State-Machine** | Nên dùng cho kiểm tra trường bắt buộc, quyền truy cập, mã tòa nhà, quy tắc SLA và định tuyến theo danh mục cố định. Khó bao phủ hết cách diễn đạt đa dạng khi phân tích ý nghĩa phản ánh. |
| **LLM Feature** | Phù hợp để tạo tóm tắt, trích xuất chủ đề và đưa ra nhãn gợi ý từ văn bản tự do. Kết quả phải có cấu trúc, giới hạn vào taxonomy được phê duyệt và được người dùng kiểm tra. |
| **Agentic Loop** | Không phù hợp giai đoạn đầu: tự tra cứu nhiều hệ thống, liên hệ cư dân hoặc chuyển/đóng ticket làm tăng rủi ro hành động sai, khó kiểm toán và chưa có bằng chứng cần thiết. |

### Future-State Flow

```text
┌────────────────────┐    ┌────────────────────┐    ┌─────────────────────┐
│ 1. Tiếp nhận       │    │ 2. Kiểm tra dữ liệu│    │ 3. 🔵 LLM hỗ trợ    │
│ phản ánh           │ →  │ và loại thông tin  │ →  │ Tóm tắt, chủ đề,    │
│                    │    │ nhạy cảm theo policy│    │ nhãn/ưu tiên gợi ý  │
└────────────────────┘    └────────────────────┘    └─────────────────────┘
                                                              │
                                                              ▼
┌────────────────────┐    ┌────────────────────┐    ┌─────────────────────┐
│ 6. Ghi nhận/chuyển │ ←  │ 5. 🟢 Nhân viên   │ ←  │ 4. Kiểm tra schema, │
│ theo quyết định    │    │ duyệt/sửa hoặc từ chối│   │ nguồn và ngưỡng tin cậy│
│ đã duyệt           │    │ đề xuất            │    │                     │
└────────────────────┘    └────────────────────┘    └─────────────────────┘
                                                              │
                            ↩️ Fallback: dữ liệu thiếu, kết quả sai định dạng,
                            lỗi hệ thống, độ tin cậy thấp hoặc nội dung nhạy cảm
                            → không tự động chuyển; đưa vào hàng đợi xử lý thủ công.
```

### Ranh giới vận hành và fallback

- Hệ thống chỉ tạo **đề xuất**; nhân viên xác nhận chủ đề, mức ưu tiên và nơi nhận trước khi ghi/chuyển ticket.
- Định tuyến kỹ thuật dựa trên danh mục và quy tắc đã cấu hình; LLM không được tự tạo bộ phận hoặc quy trình mới.
- Không đưa thông tin nhận dạng không cần thiết vào mô hình; quyền truy cập, nhật ký kiểm toán và thời hạn lưu dữ liệu phải được thống nhất trước pilot.
- Nếu mô hình thiếu căn cứ, trả về sai cấu trúc, phát hiện trường hợp nhạy cảm/khẩn cấp hoặc có lỗi tích hợp: gắn trạng thái **cần xem xét thủ công**, giữ nguyên quy trình hiện tại và hiển thị lý do.
- Dùng tập kiểm thử định kỳ để theo dõi chất lượng theo chủ đề, ngôn ngữ, tòa nhà và các nhóm phản ánh hiếm nhưng rủi ro cao.

## Phase 5 — EVALUATE

### AI Readiness Checklist

| Câu hỏi | Đánh giá hiện tại | Việc cần làm |
|---|---|---|
| Có dữ liệu mẫu/logs sạch để test? | **Chưa xác nhận.** Chưa có quyền truy cập hay mẫu dữ liệu thực trong bài làm này. | Làm việc với chủ sở hữu dữ liệu; xác định kênh, chất lượng, quyền sử dụng, ẩn danh và tập đánh giá có nhãn. |
| Rủi ro khi AI sai có nằm trong tầm kiểm soát? | **Có thể kiểm soát trong pilot có HITL**, nhưng chưa được kiểm chứng. | Giữ duyệt thủ công; định nghĩa luồng ưu tiên sự cố khẩn cấp; kiểm thử lỗi nghiêm trọng và fallback trước khi dùng thực tế. |
| Stakeholders sẵn sàng thay đổi quy trình? | **Chưa xác nhận.** | Phỏng vấn nhân viên CSKH, ban quản lý và bộ phận tiếp nhận; đồng thuận quy trình, quyền quyết định và tiêu chí thành công. |

### Quyết định đề xuất: NOT YET

**Lý do:** Bài toán có phạm vi thử nghiệm hợp lý và LLM có thể hỗ trợ phần tóm tắt/ngôn ngữ tự do, nhưng hiện chưa có dữ liệu vận hành, baseline thời gian/chất lượng, xác nhận về quyền sử dụng dữ liệu cư dân hoặc cam kết của stakeholder. Vì vậy chưa đủ bằng chứng để quyết định GO cho triển khai prototype trên dữ liệu thật. Đây không phải kết luận rằng giải pháp không khả thi.

**Điều kiện để chuyển sang GO cho pilot giới hạn:**

1. Xác nhận với Vinhomes quy trình hiện tại, nguồn dữ liệu được phép dùng và quy định bảo mật/lưu trữ.
2. Đo baseline thời gian xử lý, tỷ lệ chuyển sai, tồn đọng và xử lý lại trên mẫu đại diện.
3. Có tập đánh giá đã ẩn danh và gán nhãn; thống nhất tiêu chí chất lượng, đặc biệt cho phản ánh khẩn cấp/nhạy cảm.
4. Có nhân viên phụ trách duyệt, cơ chế ghi nhận chỉnh sửa và quy trình fallback thủ công.
5. Chạy pilot giới hạn ở chế độ gợi ý, không tự động gửi phản hồi hay thay đổi trạng thái ticket; chỉ mở rộng nếu đạt metric đã thống nhất và không phát sinh lỗi nghiêm trọng.