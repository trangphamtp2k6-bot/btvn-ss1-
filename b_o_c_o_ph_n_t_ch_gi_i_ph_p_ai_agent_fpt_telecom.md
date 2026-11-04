# Báo cáo Phân tích Giải pháp: Nâng cấp Hệ thống Chatbot LLM lên AI Agent tại FPT Telecom

## 1. Bóc tách cơ chế kỹ thuật cốt lõi
Hệ thống Chatbot hiện tại của FPT Telecom dựa trên LLM thuần túy vận hành theo cơ chế xử lý ngôn ngữ tuyến tính: Input -> Reasoning -> Output dạng văn bản. Điểm yếu lớn nhất của mô hình này là sự cô lập hoàn toàn với hệ thống cơ sở hạ tầng thực tế. Với khả năng truy vấn cơ sở dữ liệu thuê bao ở mức 0%, Chatbot hiện tại chỉ đóng vai trò như một cỗ máy sinh văn bản theo kịch bản tĩnh, không thể nắm bắt cấu hình hay tình trạng hợp đồng thực tế của khách hàng.

Ngược lại, kiến trúc AI Agent tạo ra một hệ sinh thái tự chủ thông qua chu trình khép kín: Input -> Sense (Nhận thức) -> Plan (Lập kế hoạch) -> Tool Calling (Gọi công cụ) -> Environment Feedback (Phản hồi từ môi trường) -> Output. Điểm khác biệt bản chất nằm ở khả năng "hành động". AI Agent không chỉ giới hạn ở việc sinh văn bản mà có thể trực tiếp giao tiếp với các phần mềm quản lý thông qua API. Khả năng tương tác này giúp tỷ lệ gọi Tool kiểm tra hạ tầng đạt mức 100%, biến Agent thành một tác tử chủ động thay đổi trạng thái môi trường làm việc thay vì chỉ tư vấn suông.

## 2. Phân tích quy trình xử lý ca nghiệp vụ thực tế
Khi tiếp nhận yêu cầu: *"Kiểm tra giúp tôi đường truyền mạng dạo này chậm và hỗ trợ nâng cấp gói cước mới nhất"*, AI Agent sẽ thực thi chuỗi hành động tự chủ sau:
*   **Sense & Plan (Nhận thức và Lập kế hoạch):** Agent bóc tách yêu cầu thành hai nhiệm vụ độc lập nhưng liên đới: (1) Chẩn đoán kỹ thuật đường truyền vật lý và (2) Tư vấn, thực thi nâng cấp hợp đồng dịch vụ.
*   **Tool Calling 1 (Kiểm tra hạ tầng):** Agent tự động gọi API kết nối đến hệ thống quản lý thiết bị của FPT Telecom để đọc các thông số suy hao tín hiệu quang, lịch sử rớt mạng và tình trạng băng thông của modem đầu cuối.
*   **Environment Feedback 1:** Agent tiếp nhận dữ liệu trả về từ hệ thống hạ tầng (ví dụ: tín hiệu vật lý hoàn toàn bình thường, nguyên nhân chậm là do nhu cầu sử dụng đã vượt quá giới hạn của gói cước hiện tại).
*   **Tool Calling 2 (Xử lý hợp đồng):** Tác tử tiếp tục gọi API truy vấn hệ thống CRM để trích xuất thông tin hợp đồng hiện hữu, đồng thời lọc danh sách các gói cước nâng cấp khả dụng phù hợp với hạ tầng khu vực của khách hàng.
*   **Action & Output (Hành động và Phản hồi):** Agent tổng hợp thông tin, giải thích chính xác tình trạng kỹ thuật, đề xuất gói cước mới và cung cấp lệnh xác nhận (gọi API tạo phiếu thay đổi gói cước) để hoàn tất quy trình ngay lập tức.

## 3. Đánh giá hiệu quả vận hành và thách thức chuyển đổi
Việc áp dụng AI Agent mang lại thay đổi sâu sắc về hiệu quả vận hành. Tỷ lệ giải quyết trọn vẹn yêu cầu (FCR) được kỳ vọng tăng vọt từ 22% lên 75%. Hệ quả trực tiếp là tỷ lệ yêu cầu phải chuyển tiếp cho nhân sự con người giảm mạnh từ 78% xuống còn 25%. Bằng cách tự động hóa hoàn toàn các thao tác tra cứu và chẩn đoán, AI Agent cắt giảm tuyệt đối 53% khối lượng yêu cầu cần hỗ trợ thủ công, tương đương việc giảm tải gần 68% khối lượng ticket thực tế đổ về tổng đài viên mỗi ngày.

Tuy nhiên, quá trình nâng cấp đòi hỏi doanh nghiệp phải giải quyết hai thách thức kỹ thuật cốt lõi:
1.  **Bảo mật và Phân quyền truy cập hệ thống (System Integration & Security):** Việc cấp quyền cho một tác tử AI tự động gọi API vào hệ thống hạ tầng mạng và can thiệp thay đổi gói cước trên CRM mang rủi ro lớn. Doanh nghiệp cần xây dựng cơ chế xác thực danh tính khách hàng (Authentication) nghiêm ngặt để ngăn chặn việc thay đổi dữ liệu hợp đồng trái phép.
2.  **Tối ưu hóa độ trễ phản hồi (Latency Optimization):** Chatbot cũ chỉ mất 3 giây để sinh văn bản. Tuy nhiên, AI Agent phải thực hiện thêm quy trình lập kế hoạch, liên tục gọi đa công cụ và chờ dữ liệu phản hồi từ nhiều máy chủ khác nhau. Việc duy trì thời gian phản hồi toàn trình dưới 5 giây đòi hỏi năng lực xử lý bất đồng bộ (asynchronous processing) xuất sắc từ hạ tầng máy chủ của FPT Telecom.