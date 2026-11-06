# Học viện Ôn thi 12 – V4

## Website
- `index.html` chạy trực tiếp trên GitHub Pages và giữ phần thi Lịch sử, Vật lí có sẵn.
- Mục Quản trị hiển thị Sửa / Ẩn / Hiện / Xóa ngay bên cạnh từng đề.
- Mục Tạo đề: (1) trích xuất cục bộ câu hỏi thật từ tài liệu; (2) AI tạo đề mới khi có máy chủ.
- Đề mới và thao tác quản trị đang lưu theo **từng trình duyệt**, chưa đồng bộ cho nhóm.
- **Không** tự công bố câu và đáp án AI trước khi người quản trị đọc và duyệt.

## Cấu hình AI an toàn (cần người quản trị máy chủ)
1. Người quản trị đủ điều kiện tạo một Cloudflare Worker dùng mã tại `worker/src/index.js`.
2. Triển khai Worker qua Cloudflare Dashboard hoặc Wrangler theo tài liệu chính thức.
3. Thêm **hai secret ở Worker**, không để trong GitHub Pages:
   - `OPENAI_API_KEY`: khóa API của tài khoản quản trị, dùng theo điều khoản dịch vụ.
   - `ADMIN_TOKEN`: mã truy cập ngẫu nhiên, dài và khó đoán (khác mã PIN cục bộ của website).
4. Mở website → Quản trị → Tạo đề, dán `https://<worker-domain>/generate` vào trường URL, nhập mã truy cập Worker rồi nhấn **Tạo đề bằng AI**.
5. Kiểm tra từng câu, đáp án, đơn vị, phép tính trong bản nháp trước khi duyệt.

**Quan trọng:** API có thể phát sinh phí và không bảo đảm mọi đáp án chính xác. Người dùng không cần đưa khóa API cho ChatGPT. Không dán khóa trong HTML hay vào tin nhắn. Với file chứa dữ liệu cá nhân, cần xin phép trước khi gửi cho dịch vụ AI. Worker chỉ cho phép truy cập từ `https://haodonco.github.io` nhưng không thay thế xác thực quản trị thực sự.

Nếu chưa triển khai Worker, chế độ **trích xuất cục bộ** vẫn dùng được với đề có câu hỏi sẵn, nhưng file chỉ có ma trận thường **không đủ nội dung để sinh đề mới**.