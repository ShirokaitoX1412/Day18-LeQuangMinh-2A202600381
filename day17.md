# Day 17 Submission

**Student:** Lê Quang Minh - 2A202600381  
**Date:** 24/04/2026

**Product idea:** AgeTrace (working name) là công cụ hỗ trợ điều tra giúp truy vết danh tính bằng nhận diện khuôn mặt “xuyên thời gian” (age-robust recognition) và gợi ý hồ sơ nghi vấn, kèm cơ chế kiểm soát sai số/audit để hạn chế false positive và lạm dụng giám sát.

## 1. MVP Boundary Sheet

**Riskiest Assumption:**
- Điều tra viên/cán bộ vận hành sẽ chấp nhận thêm bước “tra cứu + xác minh AI” vào workflow (trước khi ra quyết định nghiệp vụ), thay vì chỉ dựa vào nhận dạng thủ công hoặc các hệ thống tra cứu hiện có.

**In-Scope (tối đa 3):**
- Nhập 1 ảnh khuôn mặt/đoạn cắt khuôn mặt và trả về danh sách 5-20 nghi vấn (top matches) để test giả định: “có nhu cầu tra cứu nhanh”
- Hiển thị kết luận sơ bộ theo mức tin cậy (ví dụ: High/Medium/Low) để test giả định: “một kết luận ngắn gọn đủ tạo giá trị ban đầu”
- Hiển thị 1-3 lý do/điều kiện chất lượng (độ rõ, góc mặt, ánh sáng, độ lệch tuổi ước tính) + cảnh báo rủi ro sai số để test giả định: “giải thích ngắn giúp người dùng biết có nên tin kết quả”

**Out-of-Scope:**
- Quét trực tiếp từ camera thời gian thực diện rộng (mass scan) — lý do bỏ: rủi ro pháp lý/đạo đức cao, chưa cần cho MVP
- Kết nối liên quốc gia / liên cơ quan “toàn cầu” — lý do bỏ: phụ thuộc pháp lý và tích hợp quá lớn
- Tự động ra quyết định cưỡng chế (bắt/giữ/khóa) — lý do bỏ: không phù hợp, rủi ro nghiêm trọng
- Dự đoán khuôn mặt tương lai xa (15-20 năm) để match tự động — lý do bỏ: sai số cao, dễ false positive

**Non-Goals:**
- Không thay thế thẩm định của con người
- Không coi “age progression” là bằng chứng định danh
- Không chạy giám sát đại trà
- Không hứa hẹn “không bao giờ sai”
- Không lưu trữ video thô nếu không cần thiết (ưu tiên tối thiểu hóa dữ liệu)

## 2. PRD Skeleton

### Problem Statement
- Khi truy tìm người mất tích/nghi phạm sau nhiều năm, dữ liệu ảnh hiện tại thường khác xa ảnh trong hồ sơ cũ; việc đối chiếu thủ công chậm và dễ bỏ sót, trong khi các hệ thống nhận diện hiện tại có thể suy giảm độ chính xác khi khuôn mặt thay đổi theo tuổi, điều kiện chụp, hoặc chất lượng camera.

### Target User
- Điều tra viên, cán bộ truy vết, đơn vị quản lý dữ liệu sinh trắc (biometrics) trong phạm vi một cơ quan/tổ chức (ưu tiên triển khai nội bộ, có kiểm soát truy cập).

### User Stories

**Story 1:**
- As a điều tra viên, I want tra cứu nhanh từ 1 ảnh cũ/ảnh hiện tại để nhận danh sách nghi vấn trong vài phút, so that tôi rút ngắn thời gian khoanh vùng.

**Story 2:**
- As a điều tra viên, I want biết ngắn gọn vì sao kết quả “tạm tin được” hoặc “không đủ dữ liệu”, so that tôi hiểu rủi ro sai và chọn bước xác minh phù hợp (bằng chứng độc lập).

### AI-Specific

**Model Selection:**
- Model: pipeline nhận diện khuôn mặt (face detection + embedding) + mô-đun hỗ trợ “age-robust” (ví dụ: age-invariant embedding hoặc age-conditioned re-ranking) + mô-đun giải thích ngắn gọn (LLM chỉ để mô tả/giải thích, không quyết định định danh)
- Lý do chọn: MVP cần trả top matches và lý do chất lượng/rủi ro; không cần agent phức tạp hay tự động hành động ngoài hệ thống
- Trade-offs chấp nhận: độ phủ dữ liệu giới hạn, chỉ dùng trong phạm vi dataset được cấp quyền; ưu tiên minh bạch rủi ro hơn là “kết luận chắc”
- Trade-offs không chấp nhận: khẳng định danh tính khi score thấp; khuyến nghị hành động cưỡng chế; bỏ qua cảnh báo chất lượng ảnh/độ lệch tuổi

**Data Requirements:**
- Nguồn: dataset nội bộ được cấp quyền (ảnh hồ sơ, ảnh đối tượng truy vết, metadata tối thiểu); bộ quy tắc nội bộ về ngưỡng tin cậy và quy trình xác minh
- Owner: đơn vị vận hành hệ thống + đội quản trị dữ liệu
- Update frequency: theo đợt nhập hồ sơ mới và định kỳ đánh giá lại sai số (audit)

**Fallback UX:**
- Chiến lược: Expectation Management + Safe Defaults
- Trigger: ảnh mờ, góc mặt xấu, che mặt, độ lệch tuổi lớn, hoặc score thấp/không ổn định
- Hành động: hiển thị “Không đủ dữ liệu để kết luận” hoặc “Chỉ dùng để tham khảo”, kèm 1-3 lý do cụ thể + đề xuất bước cải thiện (cần ảnh rõ hơn / thêm ảnh / xác minh thủ công)
- User options: nhập thêm ảnh, giới hạn phạm vi dataset, hoặc chuyển quy trình xác minh thủ công

### Success Metrics
- Primary metric: tỷ lệ truy vấn tạo ra “case follow-up” hợp lệ (người dùng lưu lại nghi vấn + tạo ticket xác minh) trong vòng 7 ngày
- Ngưỡng thành công: có tỷ lệ đáng kể người dùng quay lại dùng lần 2 cho case khác, chứng minh đây là bước thật trong workflow
- Timeframe đo lường: 7-14 ngày sau triển khai pilot

### Dependencies & Constraints
- Ràng buộc pháp lý/quy định nội bộ về dữ liệu sinh trắc và chia sẻ dữ liệu
- Yêu cầu audit trail và phân quyền truy cập nghiêm ngặt
- Dữ liệu ảnh không đồng nhất chất lượng (camera, môi trường, năm chụp)
- Cần cơ chế đo và giảm false positive (đặc biệt khi dùng cho mục tiêu an ninh)

## 3. Hypothesis Table

**Hypothesis 1 (cho tính năng In-Scope #1):**
- "Chúng tôi tin rằng tra cứu nhanh từ 1 ảnh sẽ giúp điều tra viên rút ngắn thời gian khoanh vùng nghi vấn khi khuôn mặt thay đổi theo tuổi."
- "Chúng tôi sẽ biết mình đúng khi thấy tỷ lệ người dùng quay lại tra cứu lần thứ 2 trong 7-14 ngày và số case follow-up hợp lệ tăng trong giai đoạn pilot."

Riskiest assumption: Người dùng coi AI tra cứu là bước đáng thêm vào workflow trước khi ra quyết định nghiệp vụ.  
Cách test cheapest: MVP cho phép nhập ảnh → trả top matches + mức tin cậy + lý do chất lượng; đo tỷ lệ follow-up và quay lại dùng.

**Hypothesis 2 (nếu có):**
- "Chúng tôi tin rằng hiển thị 1-3 lý do/cảnh báo sẽ giảm việc người dùng hiểu nhầm kết quả AI là định danh chắc chắn."

Riskiest assumption: Giải thích ngắn gọn đủ để người dùng hành xử an toàn, không cần báo cáo dài.  
Cách test cheapest: A/B giữa bản chỉ có score và bản có score + cảnh báo + lý do; đo tỷ lệ hành vi rủi ro (ví dụ: lưu/đẩy case dù score thấp).

## 4. PMF Scorecard

**Aha Moment:**
- Người dùng chủ động quay lại tra cứu lần thứ hai cho một case khác và tạo follow-up xác minh dựa trên kết quả có cảnh báo rõ ràng.

**Actionable Metric:**
- Tỷ lệ người dùng thực hiện ít nhất 2 truy vấn trong 7-14 ngày và tạo ít nhất 1 follow-up hợp lệ.

**PMF Method:**
- Aha Moment tracking
- Ngưỡng thành công: có đủ người dùng quay lại và tạo follow-up để chứng minh đây không phải hành vi thử cho biết.

**Vanity Metrics tôi sẽ không dùng:**
- Số lượt đăng nhập
- Tổng số truy vấn thô
- Thời gian ở lại trang
- Số lần xem kết quả nhưng không follow-up

## 5. AI Critique Log

**Điểm AI tự phê bình:**
- Risk “function creep” cao nếu scope mơ hồ — Action: Accept — Lý do: đã giới hạn MVP ở tra cứu theo yêu cầu, không mass scan
- Dễ bị hiểu nhầm là định danh chắc chắn — Action: Accept — Lý do: thêm fallback UX + cảnh báo + yêu cầu human-in-the-loop
- Thiếu metric về sai số — Action: Partial — Lý do: MVP đo hành vi + follow-up; giai đoạn sau bổ sung đo false positive theo nhóm/điều kiện

**Thay đổi lớn nhất giữa Version A và Version B (dự kiến):**
- Siết chặt quy trình audit trail và phân quyền; bổ sung báo cáo sai số theo điều kiện ảnh và độ lệch tuổi.

## 6. Self-assessment

**Mắt xích nào trong [MVP Boundary PRD Hypothesis PMF] bạn đang yếu nhất?**
- PMF và đo “đúng sai” của hệ thống trong bối cảnh nhạy cảm: cần định nghĩa rõ thế nào là follow-up hợp lệ và cách kiểm soát false positive khi triển khai pilot.

**Open questions bạn muốn giải đáp tiếp:**
- Trong thực tế, người dùng tin dạng bằng chứng nào nhất để follow-up: ảnh so sánh, score, hay quy trình xác minh kèm audit trail?
- MVP nên bắt đầu từ object nào để sắc nhất: người mất tích (nhân đạo) hay nghi phạm (nhạy cảm hơn nhưng ROI rõ)?

