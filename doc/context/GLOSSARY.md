# GLOSSARY — Bảng thuật ngữ dùng chung NCKH_1

## 1. Thông tin tài liệu

### 1.1. Metadata

| Thuộc tính         | Giá trị                                                                                                   |
| :----------------- | :-------------------------------------------------------------------------------------------------------- |
| Tên tài liệu       | Bảng thuật ngữ dùng chung (_Glossary_)                                                                    |
| Mã tài liệu        | GLOSSARY                                                                                                   |
| Dự án              | Nền tảng tích hợp hỗ trợ tổ chức đào tạo (NCKH_1)                                                          |
| Phiên bản          | v0.1.0                                                                                                     |
| Trạng thái         | Draft                                                                                                      |
| Người viết         | Hiếu                                                                                                       |
| Người duyệt        | (chưa duyệt)                                                                                               |
| Ngày tạo           | 2026-06-13                                                                                                 |
| Ngày cập nhật      | 2026-06-13                                                                                                 |
| Tài liệu liên quan | [`../../CLAUDE.md`](../../CLAUDE.md), [`../SDLC/00-quy-chuan.md`](../SDLC/00-quy-chuan.md), [`../SDLC/01-srs.md`](../SDLC/01-srs.md), [`DOMAIN-MAP.md`](DOMAIN-MAP.md), [`../SDLC/10-architecture-decision-record.md`](../SDLC/10-architecture-decision-record.md) |

### 1.2. Lịch sử thay đổi (Changelog)

| Phiên bản | Ngày       | Tác giả | Thay đổi                                                                       |
| :-------- | :--------- | :------ | :----------------------------------------------------------------------------- |
| 0.1.0     | 2026-06-13 | Hiếu    | Tách bảng thuật ngữ từ `01-srs.md §18` thành tài liệu dùng chung (theo ADR-002). |

---

Đây là **nguồn sự thật duy nhất** cho thuật ngữ của dự án NCKH_1 (tinh thần [`00-quy-chuan.md §11`](../SDLC/00-quy-chuan.md#11-quy-ước-thuật-ngữ)). Các tài liệu khác **liên kết** về đây thay vì nhân bản định nghĩa. Thuật ngữ kỹ thuật giữ nguyên tiếng Anh, kèm chú thích lần đầu.

---

## 2. Thuật ngữ học vụ

**Học phần** (_Course_) — đơn vị kiến thức được giảng dạy, có mã, tên, số tín chỉ, mô tả, học phần tiên quyết. Một học phần có thể được mở thành nhiều _lớp học phần_ trong mỗi học kỳ.

**Lớp học phần** (_Course offering_) — một lần mở học phần X trong học kỳ Y với giảng viên, phòng học, thời gian xác định. Sinh viên đăng ký vào _lớp học phần_, không phải _học phần_.

**Học kỳ** (_Semester_) — đơn vị thời gian đào tạo; thông thường 1 năm học có 2–3 học kỳ (bao gồm học kỳ hè).

**Tín chỉ** (_Credit_) — đơn vị đo khối lượng học tập; 1 tín chỉ tương đương ~15 tiết lý thuyết hoặc ~30 tiết thực hành.

**Chương trình đào tạo** (_Curriculum_) — danh sách học phần bắt buộc và tự chọn mà sinh viên phải hoàn thành để tốt nghiệp một ngành cụ thể.

**Tiên quyết** (_Prerequisite_) — học phần A là tiên quyết của B nếu sinh viên phải hoàn thành (đạt) A trước khi đăng ký B.

**GPA** — _Grade Point Average_ (điểm trung bình tích luỹ); tính theo thang điểm 4.

**Tình trạng học vụ** — phân loại: bình thường, cảnh báo lần 1, cảnh báo lần 2, buộc thôi học; căn cứ theo quy chế đào tạo của trường.

**CVHT** — Cố vấn học tập (_Academic advisor_); vai trò bổ sung gắn trên tài khoản giảng viên.

**Tiết học** (_Slot_) — đơn vị thời gian nhỏ nhất trong thời khoá biểu; thường kéo dài 45–50 phút. Một buổi học có thể gồm 2–3 tiết liên tiếp.

**Thang điểm 10 / Thang điểm 4** — hai thang điểm phổ biến ở Việt Nam. Thang 10: điểm từ 0–10 (thường dùng khi nhập điểm từng thành phần). Thang 4: điểm từ 0.0–4.0 dùng để tính GPA theo tín chỉ. Quy đổi theo bảng quy chế của từng trường.

**Học phần tương đương** (_Equivalent course_) — học phần B được công nhận thay thế cho học phần A trong chương trình đào tạo (sinh viên chuyển ngành, chuyển trường).

**Trạng thái kế hoạch học tập** — vòng đời kế hoạch: `draft` (nháp, chưa gửi) → `submitted` (đã gửi CVHT) → `approved` (được duyệt) → `rejected` (trả lại). Kế hoạch `approved` mới được dùng để thực hiện đăng ký chính thức.

---

## 3. Thuật ngữ kỹ thuật & AI

**Verifier** — hàm kiểm tra thuần (_pure function_) nhận vào 1 lịch đề xuất và trả về danh sách ràng buộc bị vi phạm (nếu có). Không tìm lời giải, chỉ kiểm tra. Phân biệt với _solver_.

**Solver** (_Constraint solver_) — thuật toán tìm kiếm tạo ra lời giải thoả ràng buộc (OR-Tools, MiniZinc, ...). Bị cấm trong luồng suy luận chính của γ ([`BR-008`](../SDLC/01-srs.md#14-business-rules)).

**VACS** — _Vietnamese Academic Constraint Set_; bộ dữ liệu benchmark cho việc đánh giá năng lực sinh lịch AI, gồm ≥ 300 examples với gold solution.

**RAG** — _Retrieval-Augmented Generation_; kỹ thuật AI kết hợp truy vấn tài liệu nội bộ với sinh văn bản.

**CoT** — _Chain-of-Thought_; kỹ thuật nhắc mô hình suy luận từng bước.

**Idempotency-Key** — chuỗi định danh duy nhất do client tạo ra và gửi kèm yêu cầu; đảm bảo rằng nếu cùng một yêu cầu được gửi nhiều lần (do mạng bất ổn), hệ thống chỉ xử lý một lần duy nhất.

**Sandbox** — môi trường giả lập giao dịch tài chính; không có tiền thật, không kết nối cổng thanh toán thật. Mọi giao dịch đều là dữ liệu mô phỏng.

**PII** — _Personally Identifiable Information_ (thông tin nhận dạng cá nhân); bao gồm: họ tên, mã số sinh viên, ngày sinh, địa chỉ, số điện thoại, email. Cần bảo vệ theo quy định bảo vệ dữ liệu cá nhân.

---

## 4. Tham chiếu

- [`../SDLC/01-srs.md`](../SDLC/01-srs.md) — tài liệu yêu cầu phần mềm sử dụng các thuật ngữ này.
- [`../SDLC/00-quy-chuan.md`](../SDLC/00-quy-chuan.md) — §11 quy ước thuật ngữ.
- [`DOMAIN-MAP.md`](DOMAIN-MAP.md) — bản đồ miền & ma trận truy ngược.
- [`../SDLC/10-architecture-decision-record.md`](../SDLC/10-architecture-decision-record.md) — ADR-002 (lý do tách glossary ra context).
