# BÀN GIAO VẬN HÀNH NGÂN HÀNG ĐỀ HAOEDU

**Ngày lập:** 17/11/2026  
**Bên giao:** Quản lý dự án HaoEdu  
**Bên nhận:** Đồng nghiệp phụ trách nội dung tại cơ sở khác (chưa cung cấp tài khoản GitHub)  
**Trạng thái:** Tài liệu đã chuẩn bị; chưa cấp quyền chỉnh sửa hay chuyển quyền sở hữu.

## 1. Phạm vi tiếp nhận

- Soạn đề mới từ đề cương/ma trận/bản đặc tả/nguồn học liệu được phép sử dụng; đối chiếu với phạm vi và mức độ của tài liệu.
- Rà soát kiến thức, giải độc lập, sửa đáp án và lời giải; không coi đáp án do AI sinh là đã được xác minh.
- Chỉnh sửa, ẩn, hiện, thay thế hoặc xóa đề trên kho đề chung **sau khi được chủ repo chấp thuận**.
- Kiểm tra giao diện làm bài/chấm điểm và đưa bản đã duyệt lên GitHub Pages.
- Không quản trị tài khoản hay thu thập thông tin cá nhân người học nếu không có phân công riêng.

## 2. Hệ thống hiện tại (đã kiểm tra 17/11/2026)

- Website: https://haodonco.github.io/-n-t-p-l-ch-s-12/
- Repository: https://github.com/HaoDonCo/-n-t-p-l-ch-s-12
- Nhánh xuất bản: `main`; repository hiện **Public** (công khai mã nguồn).
- `index.html`: giao diện và dữ liệu đề dự phòng dạng `<script id="fallback-data" type="application/json">`.
- `data/exams.json`: kho đề ưu tiên khi website đang online. Đây là nơi chỉnh dữ liệu chính.
- Đang có **7 đề** thuộc **5 môn**: Lịch sử, Vật lí, Hóa học, Tin học, Công nghệ Lâm nghiệp.
- **Chưa có** quyền quản trị máy chủ/đồng bộ lịch sử điểm chung; kết quả và phần biên tập trên thiết bị lưu trong trình duyệt.
- Phần **Biên tập đề** đã ẩn khỏi giao diện học sinh theo yêu cầu chủ dự án. Quản trị đề chung qua GitHub, không qua màn hình học sinh.
- Cloudflare Workers AI không nằm trong luồng tạo đề chính; ưu tiên người biên soạn và kiểm tra đáp án trước khi đăng.

## 3. Quy trình nhận yêu cầu và soạn đề

1. Nhận đủ tài liệu gốc, môn, phạm vi bài, bộ sách, cấu trúc, thời gian, mức độ, thang điểm và các giới hạn của đề.
2. Lập bảng đối chiếu câu hỏi với từng yêu cầu cần đạt, cấp độ và phần thi; không suy diễn rằng một ma trận chỉ có cấu trúc đã đủ để tự tạo đáp án đáng tin cậy.
3. Soạn câu hỏi mới với bối cảnh thực tế phù hợp. Nếu tham khảo đề trên Internet, chỉ dùng để học cách hỏi/đánh giá; không chép lại nội dung có bản quyền và không tuyên bố câu tự biên soạn là “câu xuất hiện tại nhiều trường” nếu không có bằng chứng.
4. Rà soát hai lượt: (a) kỹ thuật — đủ câu, 4 phương án/4 nhận định, đáp án và đơn vị; (b) học thuật — công thức, lập luận, phép tính, từ ngữ chính xác, không có hai đáp án đúng không chủ đích.
5. Với câu tự luận trên website, hiện chỉ chấm **đáp số số học** (với dung sai), không tự động chấm lập luận bằng văn bản.
6. Gắn trạng thái `draft: true` và `verified: false` cho bản nháp chưa duyệt; chỉ bỏ `draft` khi đã qua vòng rà soát cuối.
7. Tạo pull request cho chủ dự án kiểm tra; chỉ gộp vào `main` sau khi đồng ý.

## 4. Cấu trúc dữ liệu cần tuân thủ

Mỗi đề trong `exams` có:

- `id`: chuỗi duy nhất, ổn định (không trùng đề khác).
- `subject`: `history`, `physics`, `chemistry`, `informatics`, `forestry` (môn mới cần bổ sung giao diện).
- `title`, `description`, `source`, `mode` (`exam` hoặc `mastery`), `minutes`.
- `mc`: danh sách `{ "q": "...", "options": ["A...", "B...", "C...", "D..."], "key": "B", "explain": "..." }`.
- `tf`: danh sách `{ "passage": "...", "claims": [ {"t": "...", "key": true}, {"t": "...", "key": false}, ... ] }` — luôn đủ 4 nhận định.
- `short`: `{ "q": "...", "key": 42, "unit": "...", "tol": 0.01 }` (đáp số là số).
- `essay`: `{ "title": "...", "prompt": "...", "parts": [{"q": "...", "key": 1.5, "unit": "...", "tol": 0.01, "points": 0.75}] }`.
- `sectionPoints` nếu cần: ví dụ `{ "mc": 3, "tf": 4, "short": 0, "essay": 0 }` chỉ phản ánh **phần được chấm trên web**. Không biến điểm trắc nghiệm quy đổi thành điểm toàn bài nếu đề chính thức còn tự luận.
- `active: false` để ẩn đề khỏi danh sách công khai; `active: true` hoặc bỏ thuộc tính để hiện. `draft: true` để giữ bản nháp chưa xuất bản.
- `verified: true` **chỉ** khi đã có kiểm tra nội dung và đáp án độc lập. Đây là nhãn dữ liệu, không thay thế kiểm duyệt thực tế.

## 5. Cập nhật trên GitHub

1. Mở `data/exams.json`; thêm/sửa đúng đề bằng `id`, giữ nguyên các đề khác.
2. Validate JSON, đếm câu từng phần, đảm bảo khóa đáp án hợp lệ.
3. **Đồng bộ** dữ liệu dự phòng bên trong thẻ `fallback-data` của `index.html`. Nếu không, bản HTML mở offline hoặc khi tải dữ liệu lỗi có thể hiện đề cũ.
4. Thay đổi qua nhánh riêng và pull request (không sửa vội trực tiếp `main`).
5. Sau khi gộp, kiểm tra GitHub Pages và mở trang làm bài ở chế độ ẩn danh hoặc Ctrl+F5.
6. Với yêu cầu **ẩn**, ưu tiên `active:false` thay vì xóa hẳn. Với yêu cầu **xóa**, sao lưu bản đề cũ rồi mới loại khỏi kho và fallback.
7. Lưu nhật ký: ngày, đề, người thực hiện, nguồn, nội dung thay đổi, người duyệt.

## 6. Danh sách kiểm tra nghiệm thu (phải đạt)

- [ ] Đề bám đúng ma trận/số lượng câu và nguồn tài liệu.
- [ ] Mỗi ABCD có đủ 4 phương án, đúng 1 phương án; mỗi Đ/S có 4 nhận định và khóa Boolean đúng.
- [ ] Đáp số, đơn vị, dung sai có căn cứ; tính lại bằng hướng giải độc lập.
- [ ] Các câu vận dụng thực tế có dữ kiện đủ, không mâu thuẫn.
- [ ] Chế độ `exam`: bỏ trống không được cộng điểm. Kiểm thử **0 câu đúng → 0 điểm**; làm đúng 2 câu không thể được 10.
- [ ] Chế độ `mastery`: chọn sai giữ nguyên câu, đúng mới qua; phần Đ/S phải đủ bốn ý.
- [ ] Xáo trộn từng phần, phương án ABCD và thứ tự nhận định Đ/S vẫn bảo toàn đáp án.
- [ ] Xem lại câu đúng/sai, chế độ sáng/tối, màn hình điện thoại và âm thanh bật/tắt hoạt động.
- [ ] `data/exams.json` và `fallback-data` trong `index.html` đồng bộ.
- [ ] Có người duyệt cuối trước khi xuất bản, nhất là câu Vật lí/Hóa học tính toán.

## 7. Phân quyền và bảo mật

- Chủ repo vào **Settings → Collaborators → Add people**, mời đúng tài khoản GitHub của người nhận; ưu tiên quyền chỉnh sửa mã nguồn đủ dùng, **không cấp Owner/Admin** nếu không cần.
- Repository hiện công khai; không đăng dữ liệu học sinh, thông tin đăng nhập, mã `ADMIN_TOKEN`, token Cloudflare/API hoặc thông tin nội bộ lên GitHub/Issue/PR.
- Mỗi người dùng tài khoản GitHub riêng, bật xác thực hai bước; không chia sẻ mật khẩu.
- Giữ toàn quyền gộp pull request và công bố đề ở chủ dự án hoặc người được chỉ định.
- Việc mời cộng tác viên là một thao tác riêng, **chưa được thực hiện** trong đợt chuẩn bị bàn giao này.

## 8. Mẫu yêu cầu gửi cho người phụ trách nội dung

> **Môn:** … / **Lớp:** … / **Bộ sách:** …  
> **Nguồn:** file ma trận …; file nội dung …; đề tham khảo …  
> **Cấu trúc:** … câu ABCD, … nhóm Đ/S, … trả lời ngắn, … tự luận  
> **Mức độ:** …% nhận biết, …% thông hiểu, …% vận dụng  
> **Thời gian:** … phút / **Thang điểm:** …  
> **Yêu cầu:** mỗi câu có đáp án và giải thích; kiểm tra độc lập; lưu bản nháp để duyệt trước khi đăng  
> **Ngày cần xuất bản:** … / **Người duyệt:** …

## 9. Xác nhận bàn giao

- Người nhận: ___________________ (GitHub: ___________________)
- Ngày tiếp nhận: ___________________
- Quyền truy cập được cấp: ___________________
- Đã thử cập nhật một đề ở nhánh riêng và tạo pull request: Có / Chưa
- Chủ dự án xác nhận nghiệm thu: ___________________

*Tài liệu này không chứa khóa bí mật và không xác nhận rằng quyền truy cập đã được chuyển giao.*
