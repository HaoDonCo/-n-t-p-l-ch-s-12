# Hướng dẫn cập nhật lên GitHub (`HaoDonCo/-n-t-p-l-ch-s-12`)

Gói cập nhật này đã thiết lập đầy đủ 2 chức năng chính cho website **HaoEdu**:
1. **Làm bài thi trắc nghiệm kép (Tự động tạo 2 đề cho mỗi tài liệu gửi lên)**:
   - **Đề 1 (`mode: "mastery"`)**: Xuất hiện từng câu một, học sinh làm đúng mới qua câu tiếp theo.
   - **Đề 2 (`mode: "exam"`)**: Hiển thị toàn bộ câu hỏi, làm xong nộp bài và chấm điểm 1 lần.
   - Đã tạo sẵn cặp 2 đề cho **Bài 4 (Chỉnh lưu dòng điện xoay chiều)** và **Bài 5 (Tia X)** trong `data/new_dual_exams_bai4_bai5.json`.
2. **Lưu trữ Bài thuyết trình (`presentations/` & `data/presentations.json`)**:
   - Đã tích hợp **Bài 4: Chỉnh lưu dòng điện xoay chiều** (`presentations/bai-4-chinh-luu-dong-dien-xoay-chieu.html`) kèm ảnh đại diện SVG.
   - Đã tích hợp **Bài 5: Tia X** (`presentations/bai-5-tia-x.html`) kèm ảnh đại diện SVG.

## Cách đẩy lên GitHub nhanh nhất (Giữ nguyên 100% các đề cũ trong `data/exams.json`)
1. Giải nén gói `haoedu_github_update.zip` đè vào thư mục repo `-n-t-p-l-ch-s-12` trên máy của bạn.
2. Chạy lệnh đồng bộ để ghép 7 đề cũ trong `data/exams.json` với cặp đề mới và nhúng vào `index.html`:
   ```bash
   python scripts/sync_dual_exams.py
   ```
3. Commit và đẩy lên GitHub:
   ```bash
   git add index.html data/ presentations/ scripts/
   git commit -m "Nang cap 2 chuc nang chinh: Cap 2 de Mastery/Exam & Kho Bai thuyet trinh Bai 4, Bai 5"
   git push origin main
   ```
