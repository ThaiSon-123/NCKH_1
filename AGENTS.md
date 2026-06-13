# AGENTS.md — Hướng dẫn cho Codex làm việc trong kho mã này

Tài liệu này dành cho Codex (và các agent dẫn xuất) khi làm việc trong kho mã của
dự án **Nền tảng tích hợp hỗ trợ tổ chức đào tạo đại học có ứng dụng AI**
(tên gọi nội bộ: _NCKH_1_).

Đọc trước tiên — bắt buộc:

1. Phần "Tóm tắt dự án" và "Quy tắc làm việc cốt lõi" trong file này.
2. [`doc/SDLC/00-quy-chuan.md`](doc/SDLC/00-quy-chuan.md) — quy chuẩn viết tài liệu &
   xây dựng phần mềm áp dụng cho toàn dự án.
3. [`doc/agents.md`](doc/agents.md) — chỉ dẫn dành riêng cho agent khi soạn tài liệu
   trong thư mục `doc/`.

---

## 1. Tóm tắt dự án

### 1.1. Vấn đề

Công tác tổ chức đào tạo ở một cơ sở giáo dục đại học là quy trình nhiều bên liên
quan, ràng buộc lẫn nhau: **khoa/bộ môn** xây dựng chương trình và mở học phần,
**phòng đào tạo** tổ chức lớp học phần và quản lý học kỳ, **giảng viên** được phân
công giảng dạy, **sinh viên** đăng ký học theo chương trình của mình. Xen suốt là
các hoạt động tài chính (học phí) và trao đổi thông tin (thông báo, hỏi đáp học vụ).
Mỗi quyết định ở một khâu lại kéo theo hệ quả ở khâu khác.

Hiện trạng các công cụ hỗ trợ rời rạc, thủ công, thiếu thông minh. Có khối lượng
lớn công việc lặp lại và mang tính phân tích/giao tiếp chưa được tự động hoá.

### 1.2. Mục tiêu

Xây dựng **một nền tảng tích hợp**, lấy dữ liệu học vụ thống nhất làm trung tâm,
phục vụ đồng thời **bốn nhóm người dùng** và ứng dụng **AI** xuyên suốt để hỗ trợ
ra quyết định và giảm thao tác thủ công.

### 1.3. Sáu nhóm người dùng nghiệp vụ + vai trò kỹ thuật

| Nhóm                | Vai trò chính trong hệ thống                                                                                                                                                                                   |
| ------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Sinh viên           | Lập kế hoạch học tập, đăng ký học phần, xem học phí, hỏi đáp học vụ, khảo sát đánh giá giảng dạy.                                                                                                              |
| Phòng Đào tạo       | Tổ chức lớp học phần, theo dõi học phí, giám sát bất thường, tổng hợp đánh giá giảng dạy.                                                                                                                      |
| Khoa / Bộ môn       | Quản lý chương trình, học phần, phân công giảng dạy, theo dõi chất lượng giảng dạy của khoa.                                                                                                                   |
| Giảng viên          | Xem lớp được phân công, lịch dạy, danh sách SV, xem feedback đánh giá về mình. **Có thể kiêm Cố vấn học tập (CVHT)** — capability gắn trên GV: duyệt kế hoạch học tập, theo dõi tiến độ của SV được phân công. |
| Phòng Khảo thí      | Lập lịch thi, phân phòng, phân giám thị, quản lý đề thi.                                                                                                                                                       |
| Lãnh đạo cấp trường | **Chỉ đọc (read-only)** dashboard tổng hợp & báo cáo. Không cấu hình hệ thống, không nhập/sửa dữ liệu nghiệp vụ.                                                                                               |

> **Quản trị viên hệ thống (SysAdmin)** là vai trò _kỹ thuật_, không thuộc
> nhóm người dùng nghiệp vụ — quản tài khoản, phân vai trò, backup, log;
> không tác động lên dữ liệu nghiệp vụ.

### 1.4. Tám nhóm bài toán con

1. **Hỗ trợ kế hoạch học tập của sinh viên** — gợi ý môn theo chương trình & tiến
   độ; sinh thời khoá biểu tự động không trùng lịch, theo sở thích cá nhân;
   CVHT duyệt kế hoạch.
2. **Hỗ trợ tổ chức & giám sát đào tạo** — quản lý lớp/chương trình/giảng viên;
   cảnh báo sớm bất thường (quá tải lớp, trùng lịch/phòng).
3. **Hỗ trợ tài chính học vụ** — học phí theo tín chỉ; sinh viên xem/ước tính/
   thanh toán **sandbox**; phòng đào tạo theo dõi thu, cảnh báo công nợ.
4. **Hỗ trợ AI xuyên suốt** — chiến lược chung cho toàn nền tảng: gợi ý, phân
   tích/cảnh báo, sinh email–thông báo tự động _(có người duyệt)_, hội thoại
   học vụ có trích dẫn nguồn.
5. **Quản lý kết quả học tập** — nhập/duyệt/công bố điểm, bảng điểm tích luỹ,
   cảnh báo học vụ. _AI_: dự đoán sớm nguy cơ rớt môn / cảnh báo học vụ.
6. **Tổ chức khảo thí** — lịch thi không trùng SV, phân phòng & giám thị,
   quản lý đề. _AI_: sinh lịch thi tối ưu (giảm xung đột SV, cân tải phòng).
7. **Phân tích & báo cáo cho lãnh đạo** — dashboard chỉ số đào tạo (tỉ lệ tốt
   nghiệp, tải giảng dạy, hiệu suất chương trình). _AI_: phát hiện xu hướng
   bất thường + sinh báo cáo tóm tắt định kỳ.
8. **Đánh giá giảng dạy** — SV khảo sát cuối kỳ; GV xem feedback về mình; Khoa
   / PĐT / Lãnh đạo tổng hợp. _AI_: phân tích cảm xúc + phân loại chủ đề bình
   luận tự do, tóm tắt feedback theo môn / giảng viên.

### 1.5. Phạm vi & ranh giới đã chốt

- Áp dụng trong **phạm vi một khoa/trường**; nền tảng hoàn chỉnh, chạy được,
  đánh giá được.
- **Thanh toán học phí chỉ ở môi trường sandbox** — không giao dịch tiền thật,
  không lưu dữ liệu tài chính thật.
- **Miễn/giảm học phí** theo chính sách thuộc _phần mở rộng tương lai_, không
  nằm trong phạm vi MVP.
- Tiêu chí thành công **không yêu cầu tính mới khoa học** mà ở chỗ:
  - Hệ thống vận hành đúng, đủ chức năng cho **sáu nhóm người dùng nghiệp vụ**
    (và vai trò kỹ thuật SysAdmin) trên **tám nhóm bài toán con**;
  - Mỗi năng lực AI có **ít nhất một chỉ số định lượng** chứng minh hiệu quả.

---

## 2. Cấu trúc kho mã

```
NCKH_1/
├── AGENTS.md                  ← bạn đang ở đây
├── README.md                  ← giới thiệu công khai (ngắn)
└── doc/
    ├── agents.md              ← chỉ dẫn cho agent khi viết tài liệu
    ├── context/               ← bối cảnh, phỏng vấn, ghi chú dự án
    └── SDLC/                  ← tài liệu vòng đời phát triển
        ├── 00-quy-chuan.md                       ← chuẩn dùng chung
        ├── 01-srs.md                             ← yêu cầu phần mềm
        ├── 02-hld.md                             ← thiết kế tổng thể
        ├── 03-lld.md                             ← thiết kế chi tiết
        ├── 04-database-design.md
        ├── 05-api-specification.md
        ├── 06-ui-ux-flow-specification.md
        ├── 07-security-permission-design.md
        ├── 08-test-plan-acceptance-criteria.md
        ├── 09-deployment-operation-standard.md
        ├── 10-architecture-decision-record.md
        └── 11-project-task-breakdown.md
```

Mã nguồn sẽ được tổ chức sau khi tài liệu thiết kế (đặc biệt là **02-hld.md** và
**03-lld.md**) đã được thống nhất. **Chưa được tạo thư mục mã nguồn trước khi có
HLD**, trừ khi người dùng yêu cầu rõ ràng.

---

## 3. Quy tắc làm việc cốt lõi

### 3.1. Ngôn ngữ

- **Tài liệu viết bằng tiếng Việt.** Thuật ngữ kỹ thuật giữ nguyên tiếng Anh khi
  không có bản dịch chuẩn (ví dụ: _use case_, _endpoint_, _rate limit_). Lần
  xuất hiện đầu tiên kèm chú thích tiếng Việt trong ngoặc.
- **Mã nguồn, định danh, commit message bằng tiếng Anh.**
- Phản hồi cho người dùng: theo ngôn ngữ người dùng đang dùng.

### 3.2. Tài liệu

- Mọi tài liệu mới hoặc chỉnh sửa trong `doc/` **phải tuân theo**
  [`doc/SDLC/00-quy-chuan.md`](doc/SDLC/00-quy-chuan.md).
- Khi cần viết một file SDLC cụ thể, **đọc trước**
  [`doc/agents.md`](doc/agents.md) để biết: thứ tự viết, các file phụ thuộc,
  nội dung tối thiểu, checklist hoàn thành.
- **Không viết tài liệu ra ngoài thư mục `doc/`** trừ khi được yêu cầu rõ
  (ví dụ `README.md` ở gốc).

### 3.3. Phạm vi thay đổi

- Bài toán này có **sáu nhóm người dùng nghiệp vụ** và **tám nhóm chức năng**
  đan xen.
  Một thay đổi tưởng nhỏ ở một khâu (ví dụ: đổi điều kiện mở lớp) có thể vô
  hiệu hoá luồng khác (ví dụ: đăng ký học của sinh viên). **Trước khi sửa, đọc
  các tài liệu liên đới** liệt kê trong `00-quy-chuan.md §traceability`.
- **Không thêm tính năng nằm ngoài phạm vi đã chốt** (xem §1.5). Cụ thể: không
  triển khai cổng thanh toán thật; không xây miễn/giảm học phí; không mở rộng
  cho nhiều trường.

### 3.4. Tính năng AI

- Mỗi năng lực AI (gợi ý môn, sinh thời khoá biểu, cảnh báo bất thường, sinh
  email, hội thoại có trích dẫn, ...) **phải đi kèm một chỉ số đánh giá định
  lượng** được mô tả rõ trong `01-srs.md` và `08-test-plan-acceptance-criteria.md`.
- Mọi nội dung AI **gửi ra ngoài** (email, thông báo) **phải qua bước người
  duyệt** trước khi phát hành. Đây là ràng buộc thiết kế cứng.
- Hội thoại học vụ **phải trích dẫn nguồn** (quy chế, biểu mẫu, học phần) —
  không được trả lời chỉ bằng "kiến thức nền" của mô hình.

### 3.5. Khi không chắc chắn

Bài toán có nhiều ràng buộc nghiệp vụ tiếng Việt (quy chế đào tạo, học phí,
học phần). **Khi gặp tình huống mơ hồ, hỏi người dùng trước khi quyết định.**
Đặc biệt với các quyết định ảnh hưởng tới schema cơ sở dữ liệu, API công khai,
hoặc luồng nghiệp vụ liên nhóm người dùng.

---

## 4. Lệnh & quy trình hay dùng

Dự án ở giai đoạn khởi tạo tài liệu — chưa có lệnh build/test/deploy. Khi mã
nguồn được thêm, mục này sẽ được cập nhật cùng `09-deployment-operation-standard.md`.

---

## 5. Ghi chú nội bộ

- **Bối cảnh đầy đủ** của dự án (vấn đề, mục tiêu, phạm vi, ranh giới) đã được
  ghi ở §1. Khi tài liệu trong `doc/context/` được bổ sung, các agent nên đọc
  nội dung đó để có thêm chi tiết nghiệp vụ.
- **Quy chuẩn cao nhất** mà mọi tài liệu phải tuân theo là
  [`doc/SDLC/00-quy-chuan.md`](doc/SDLC/00-quy-chuan.md). Khi quy chuẩn xung
  đột với hướng dẫn ở file này, quy chuẩn ưu tiên cho phần _cách viết_; còn
  _phạm vi & ranh giới dự án_ thì file này (AGENTS.md) là nguồn sự thật.
