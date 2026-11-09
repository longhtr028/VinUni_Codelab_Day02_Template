
# Phase 1 — SCAN: Danh sách bài toán vận hành

| # | Subsidiary (VinFast/Xanh SM...) | Lens | Mô tả ngắn bài toán |
|---|---|---|---|
| 1 | Vinhomes | Phát hiện bất thường, giảm lãng phí | Phân tích dữ liệu điện, nước và cảm biến tòa nhà để phát hiện sớm mức tiêu thụ bất thường, rò rỉ hoặc thiết bị vận hành kém hiệu quả; gửi cảnh báo đến đội kỹ thuật. |
| 2 | Vinhomes | Rò rỉ hiệu suất vận hành | Phân tích lịch sử yêu cầu sửa chữa để phát hiện thiết bị có nguy cơ hỏng, ưu tiên bảo trì và giảm sự cố khẩn cấp. |
| 3 | Vinhomes | Thất thoát chi phí | Đối soát hóa đơn điện, nước và phí dịch vụ với chỉ số công tơ, biểu giá và dữ liệu căn hộ để phát hiện bất thường trước khi lập hóa đơn. |
| 4 | Vinhomes | Điều phối nguồn lực | Dự báo khối lượng yêu cầu theo tòa nhà, loại sự cố và khung giờ để phân ca nhân viên kỹ thuật, vệ sinh và an ninh phù hợp. |
| 5 | Vinhomes | Phản hồi khách hàng | Tổng hợp phản ánh và khảo sát cư dân để phát hiện vấn đề lặp lại hoặc nghiêm trọng, tạo cảnh báo và bản tóm tắt ưu tiên cho ban quản lý. |

## Phase 2 — QUICK-ASSESS: 3 Quick Problem Cards

> **Lưu ý:** Thời gian và mục tiêu dưới đây là giả định ban đầu để scoping, chưa phải số liệu nội bộ đã xác minh. Cần đo baseline với ban quản lý và dữ liệu thực tế trước khi cam kết hiệu quả.

### Quick Problem Card #1 — Phát hiện bất thường điện, nước

| Trường | Nội dung |
|---|---|
| **Bài toán (1 câu)** | Phát hiện sớm mức tiêu thụ điện, nước bất thường hoặc dấu hiệu rò rỉ từ dữ liệu công tơ và cảm biến tòa nhà để đội kỹ thuật kiểm tra. |
| **Công ty thành viên** | [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  [ ] Vinmec |
| **Ai đang đau (Actor)?** | Nhân viên vận hành tòa nhà và đội kỹ thuật; cư dân có thể chịu ảnh hưởng nếu sự cố rò rỉ hoặc thiết bị kém hiệu quả không được phát hiện sớm. |
| **Workflow thủ công hiện tại (4 bước)** | 1. Theo dõi chỉ số trên hệ thống công tơ/BMS → 2. So sánh với lịch sử hoặc ngưỡng cảnh báo → 3. Xác minh bất thường với nhân viên/tòa nhà → 4. Tạo yêu cầu kiểm tra cho đội kỹ thuật. |
| **Bước tốn thời gian/lỗi nhất** | So sánh và xác minh số liệu bất thường (ước tính **15 phút/sự kiện**, cần đo thực tế). |
| **AI có thể hỗ trợ ở bước nào?** | Tự động đối chiếu mức tiêu thụ với ngưỡng và lịch sử theo thời gian; tạo cảnh báo kèm dữ liệu liên quan để nhân viên xác nhận trước khi điều phối kiểm tra. |
| **Metric thành công** | Giảm thời gian từ khi có dữ liệu bất thường đến khi tạo cảnh báo xuống **dưới 5 phút**; đạt **ít nhất 85% cảnh báo hữu ích** sau giai đoạn thử nghiệm, đồng thời theo dõi tỷ lệ cảnh báo sai. |
| **Quick Architecture** | [ ] No AI  [x] Rule  [ ] LLM  [ ] Agent |

### Quick Problem Card #2 — Đối soát hóa đơn dịch vụ

| Trường | Nội dung |
|---|---|
| **Bài toán (1 câu)** | Đối chiếu hóa đơn điện, nước và phí dịch vụ với chỉ số công tơ, biểu giá và thông tin căn hộ để phát hiện sai lệch trước khi lập hóa đơn. |
| **Công ty thành viên** | [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  [ ] Vinmec |
| **Ai đang đau (Actor)?** | Nhân viên kế toán/đối soát và ban quản lý tòa nhà; cư dân bị ảnh hưởng nếu hóa đơn sai hoặc phải điều chỉnh sau khi phát hành. |
| **Workflow thủ công hiện tại (4 bước)** | 1. Tập hợp dữ liệu công tơ và thông tin căn hộ → 2. Tra biểu giá và quy tắc tính phí → 3. So sánh với hóa đơn dự kiến, kiểm tra khoản lệch → 4. Xác minh và điều chỉnh dữ liệu trước khi phát hành. |
| **Bước tốn thời gian/lỗi nhất** | Đối chiếu từng khoản lệch giữa các nguồn dữ liệu (ước tính **10 phút/trường hợp cần kiểm tra**, cần đo thực tế). |
| **AI có thể hỗ trợ ở bước nào?** | Tự động kiểm tra các trường và phép tính theo biểu giá/quy tắc đã được phê duyệt; đưa trường hợp sai lệch vào danh sách để nhân viên xác minh. |
| **Metric thành công** | Giảm thời gian kiểm tra một trường hợp sai lệch từ **10 phút xuống dưới 3 phút**; phát hiện ít nhất **95% sai lệch đã biết** trong tập dữ liệu kiểm thử trước khi áp dụng thực tế. |
| **Quick Architecture** | [ ] No AI  [x] Rule  [ ] LLM  [ ] Agent |

### Quick Problem Card #3 — Tổng hợp phản ánh cư dân

| Trường | Nội dung |
|---|---|
| **Bài toán (1 câu)** | Tổng hợp phản ánh và khảo sát cư dân để nhận diện vấn đề lặp lại hoặc nghiêm trọng, rồi tạo bản tóm tắt ưu tiên cho ban quản lý. |
| **Công ty thành viên** | [ ] VinFast  [ ] Xanh SM  [x] Vinhomes  [ ] Vinmec |
| **Ai đang đau (Actor)?** | Nhân viên chăm sóc cư dân và ban quản lý cần đọc, phân loại, chuyển xử lý nhiều phản ánh qua các kênh khác nhau. |
| **Workflow thủ công hiện tại (4 bước)** | 1. Tiếp nhận phản ánh/khảo sát → 2. Đọc và phân loại nội dung → 3. Tìm phản ánh tương tự, xác định mức độ ưu tiên → 4. Chuyển bộ phận phụ trách và tổng hợp báo cáo. |
| **Bước tốn thời gian/lỗi nhất** | Đọc, phân loại và tổng hợp nội dung tự do (ước tính **8 phút/phản ánh**, cần đo thực tế). |
| **AI có thể hỗ trợ ở bước nào?** | LLM tóm tắt, gợi ý chủ đề và phát hiện các phản ánh có nội dung tương tự; nhân viên xác nhận mức độ ưu tiên và bộ phận nhận xử lý. |
| **Metric thành công** | Giảm thời gian phân loại và tóm tắt từ **8 phút xuống dưới 2 phút/phản ánh**; ít nhất **90% phản ánh được chuyển đúng nhóm** trong tập kiểm thử, với người phụ trách duyệt các trường hợp khẩn cấp hoặc nhạy cảm. |
| **Quick Architecture** | [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent |

