# crawl-data
1.  Quy trình cào Website Tin tức (CafeF, Investing, Reuters...)
Đây là luồng dữ liệu chính thống, ổn định nhất.

  Bước 1: Khởi tạo (Initialization): Thiết lập BrowserConfig với Proxy dân cư theo quốc gia tương ứng với nguồn tin (Ví dụ: cào tin Mỹ thì dùng IP Mỹ).

  Bước 2: Cấu hình Stealth: Kích hoạt use_stealth_js và chặn các tài nguyên không cần thiết (images, fonts, ads) để tiết kiệm băng thông proxy.

  Bước 3: Trích xuất thông minh (Extraction): Sử dụng LLMExtractionStrategy của Crawl4AI để yêu cầu AI lọc ra: Tiêu đề, Nội dung, và các thực thể tài chính (mã cổ phiếu, con số tăng trưởng).

  Bước 4: Kiểm soát lỗi: Thiết lập cơ chế tự động thử lại (Retry) với một IP khác trong pool proxy dân cư nếu gặp lỗi 403 hoặc 429.

2. Quy trình cào Facebook (Fanpage/Group)
Mục tiêu là lấy bài viết và bình luận để đo lường tâm lý đám đông (Social Sentiment).

  Bước 1: Chuẩn bị Danh tính (Identity): * Sử dụng một Nick Clone (đã nuôi lâu).

Đăng nhập thủ công trên trình duyệt qua Proxy dân cư đó để lấy Cookie.

  Bước 2: Cấu hình Sticky Proxy: Đây là bước quan trọng nhất. Anh phải đảm bảo Nick A luôn đi kèm với IP A. Nếu thay đổi IP đột ngột, Nick sẽ bị khóa (Checkpoint).

  Bước 3: Điều hướng & Cuộn (Navigation & Scroll):

Dùng Crawl4AI điều hướng đến URL Fanpage.

Sử dụng hàm js_code để thực hiện thao tác cuộn trang (Scroll) giả lập người dùng thật nhằm load thêm bài viết (Lazy Loading).

  Bước 4: Trích xuất nội dung bài đăng: Crawl4AI sẽ quét các thẻ div chứa nội dung bài viết.

  Bước 5: Chống phát hiện: Thiết lập magic_sleep (nghỉ từ 5 - 15 giây) giữa mỗi lần cuộn để qua mặt thuật toán giám sát của Meta.

3. Quy trình cào Zalo (Tường/Timeline)(Đang trong quá trình test vì api zalo khá kín )

  Bước 1: Thiết lập Session (Lần đầu):

Dựng một kịch bản Playwright (lõi của Crawl4AI) mở trang chat.zalo.me.

Anh quét mã QR thủ công. Sau đó, hệ thống sẽ lưu file Auth States (chứa tất cả session/local storage).

  Bước 2: Tái sử dụng Session: Các lần cào sau, Crawl4AI sẽ load file Auth States này để vào thẳng bên trong mà không cần đăng nhập lại.

  Bước 3: Truy cập Timeline: * Bot click vào biểu tượng Timeline hoặc điều hướng qua URL nếu có thể.

Dùng Proxy dân cư Việt Nam 100% (Zalo rất nhạy cảm với IP nước ngoài).

  Bước 4: Parsing dữ liệu: Vì cấu trúc HTML của Zalo khá đặc thù và hay thay đổi, anh nên dùng CSS Selector chính xác vào các thẻ chứa bài đăng của bạn bè/người dùng.

  Bước 5: Export: Chuyển đổi dữ liệu về JSON để đẩy vào Database chung với dữ liệu Website và Facebook.
