# DOMAIN-MAP — Bản đồ miền & truy ngược NCKH_1

## 1. Thông tin tài liệu

### 1.1. Metadata

| Thuộc tính         | Giá trị                                                                                                   |
| :----------------- | :-------------------------------------------------------------------------------------------------------- |
| Tên tài liệu       | Bản đồ miền & ma trận truy ngược (_Domain map & traceability_)                                            |
| Mã tài liệu        | DOMAIN-MAP                                                                                                 |
| Dự án              | Nền tảng tích hợp hỗ trợ tổ chức đào tạo (NCKH_1)                                                          |
| Phiên bản          | v0.2.0                                                                                                     |
| Trạng thái         | Draft                                                                                                      |
| Người viết         | Hiếu                                                                                                       |
| Người duyệt        | (chưa duyệt)                                                                                               |
| Ngày tạo           | 2026-06-13                                                                                                 |
| Ngày cập nhật      | 2026-06-13                                                                                                 |
| Tài liệu liên quan | [`../../CLAUDE.md`](../../CLAUDE.md), [`../SDLC/01-srs.md`](../SDLC/01-srs.md), [`GLOSSARY.md`](GLOSSARY.md), [`../SDLC/10-architecture-decision-record.md`](../SDLC/10-architecture-decision-record.md) |

### 1.2. Lịch sử thay đổi (Changelog)

| Phiên bản | Ngày       | Tác giả  | Thay đổi                                                                                         |
| :-------- | :--------- | :------- | :----------------------------------------------------------------------------------------------- |
| 0.2.0     | 2026-06-13 | AI Agent | Đồng bộ `01-srs.md` v0.5.0: thêm UC-017/018, FR-025/026 và cập nhật ma trận truy ngược.          |
| 0.1.0     | 2026-06-13 | Hiếu     | Tạo bản đồ miền; chuyển ma trận truy ngược từ `01-srs.md §19` về đây (theo ADR-002).             |

---

Tài liệu này là **bản đồ định hướng** miền học vụ NCKH_1: ai dùng hệ thống, hệ thống giải bài toán gì, dữ liệu xoay quanh thực thể nào, và **ma trận truy ngược** giữa các hạng mục yêu cầu. Mục đích: người mới nắm tổng thể trước khi đọc chi tiết trong [`01-srs.md`](../SDLC/01-srs.md). Định nghĩa thuật ngữ: [`GLOSSARY.md`](GLOSSARY.md).

---

## 2. Nhóm người dùng & vai trò

Chi tiết: [`01-srs.md §7`](../SDLC/01-srs.md#7-actor-và-vai-trò).

| Nhóm                   | Mã vai trò              | Vai trò chính (tóm tắt)                                              |
| :--------------------- | :---------------------- | :------------------------------------------------------------------- |
| Sinh viên              | `student`               | Lập kế hoạch, đăng ký học, xem học phí, hỏi đáp, khảo sát.            |
| Giảng viên             | `lecturer`              | Xem lớp/lịch dạy/danh sách SV, nhập điểm, xem feedback.              |
| Cố vấn học tập (CVHT)  | `academic_advisor` (\*) | Duyệt kế hoạch & theo dõi tiến độ SV phụ trách (\* capability trên `lecturer`). |
| Khoa / Bộ môn          | `department`            | Quản lý chương trình, học phần, phân công giảng dạy, chất lượng.     |
| Phòng Đào tạo          | `training_office`       | Lớp học phần, học kỳ/phòng, học phí, giám sát bất thường.            |
| Phòng Khảo thí         | `examination_office`    | Lịch thi, phân phòng & giám thị, đề thi.                             |
| Lãnh đạo cấp trường    | `leadership`            | Đọc dashboard & báo cáo (read-only).                                |
| Quản trị viên hệ thống | `sysadmin`              | Quản tài khoản, phân vai trò, backup, log (không đụng dữ liệu nghiệp vụ). |

---

## 3. Tám nhóm bài toán con

Chi tiết: [`01-srs.md §4.3`](../SDLC/01-srs.md#43-tám-nhóm-bài-toán-con).

| #   | Nhóm bài toán                     | UC chính                | Năng lực AI       |
| :-- | :-------------------------------- | :---------------------- | :---------------- |
| 1   | Hỗ trợ kế hoạch học tập sinh viên | UC-001, UC-002, UC-003  | AI-001, AI-002    |
| 2   | Tổ chức & giám sát đào tạo        | UC-004, UC-005, UC-017  | AI-003            |
| 3   | Tài chính học vụ (sandbox)        | UC-007, UC-008          | AI-008            |
| 4   | AI xuyên suốt                     | UC-015 + mọi UC có AI   | AI-001 … AI-008   |
| 5   | Quản lý kết quả học tập           | UC-009, UC-010          | AI-005            |
| 6   | Tổ chức khảo thí                  | UC-011, UC-012          | AI-006            |
| 7   | Phân tích & báo cáo cho lãnh đạo  | UC-016                  | AI-003, AI-007    |
| 8   | Đánh giá giảng dạy                | UC-013, UC-014          | AI-007            |

---

## 4. Bản đồ thực thể miền

Sơ đồ ER mức cao: [`01-srs.md §9`](../SDLC/01-srs.md#9-mô-hình-dữ-liệu-mức-cao). Schema chi tiết: [`04-database-design.md`](../SDLC/04-database-design.md) (sẽ viết). Bảng dưới mô tả vai trò từng thực thể cốt lõi.

| Thực thể            | Vai trò trong miền                                              | Nhóm bài toán |
| :------------------ | :-------------------------------------------------------------- | :------------ |
| `NGUOI_DUNG`        | Tài khoản + vai trò (gốc xác thực/phân quyền).                 | xuyên suốt    |
| `SINH_VIEN`         | Hồ sơ sinh viên, tiến độ học tập.                              | 1, 3, 5       |
| `GIANG_VIEN`        | Hồ sơ giảng viên (kiêm CVHT/giám thị).                         | 2, 5, 6, 8    |
| `KHOA`              | Đơn vị ban hành chương trình.                                  | 2             |
| `CHUONG_TRINH`      | Chương trình đào tạo (danh mục học phần, tiên quyết).          | 1, 2          |
| `HOC_PHAN`          | Học phần (đơn vị kiến thức).                                   | 1, 2          |
| `LOP_HOC_PHAN`      | Lần mở học phần trong 1 học kỳ (đối tượng đăng ký).           | 1, 2, 6, 8    |
| `HOC_KY`            | Khung thời gian đào tạo, mở/đóng đăng ký.                     | 2             |
| `PHONG_HOC`         | Phòng học/phòng thi, sức chứa.                                 | 2, 6          |
| `DANG_KY`           | Ghi danh SV vào lớp học phần.                                 | 1, 3          |
| `DIEM_SO` / `BANG_DIEM` | Điểm thành phần & bảng điểm tích luỹ, GPA.                | 5             |
| `KE_HOACH_HOC_TAP`  | Kế hoạch học tập (vòng đời draft→approved).                   | 1             |
| `HOA_DON_HOC_PHI`   | Học phí sandbox, trạng thái thanh toán.                       | 3             |
| `LICH_THI` / `CA_THI` / `GIAM_THI` / `DE_THI` | Tổ chức khảo thí.                   | 6             |
| `KHAO_SAT` / `PHAN_HOI` | Khảo sát ẩn danh & phản hồi đánh giá giảng dạy.          | 8             |
| `THONG_BAO`         | Thông báo in-app / email (AI soạn có người duyệt).           | xuyên suốt    |
| `NHAT_KY_KIEM_TOAN` | Audit log mọi thay đổi nghiệp vụ.                             | xuyên suốt    |

---

## 5. Ma trận truy ngược

> Chuyển từ `01-srs.md §19` về đây theo [`ADR-002`](../SDLC/10-architecture-decision-record.md). Đây là **bản duy trì chính thức** của ma trận truy ngược; mỗi commit thêm/đổi UC/FR **PHẢI** cập nhật bảng này. Sẽ mở rộng sau khi [`02-hld.md`](../SDLC/02-hld.md) hoàn thành.

| Use Case                                                  | Yêu cầu chức năng liên quan            | Yêu cầu phi chức năng liên quan | Yêu cầu AI liên quan                                                          |
| :-------------------------------------------------------- | :------------------------------------- | :------------------------------ | :--------------------------------------------------------------------------- |
| [UC-001](../SDLC/01-srs.md#uc-001) Gợi ý môn học          | FR-004, FR-007, FR-025                 | NFR-002, NFR-004                | [AI-001](../SDLC/01-srs.md#ai-001)                                           |
| [UC-002](../SDLC/01-srs.md#uc-002) Sinh thời khoá biểu    | FR-006, FR-007, FR-019, FR-020         | NFR-003, NFR-004, NFR-015       | [AI-002](../SDLC/01-srs.md#ai-002)                                           |
| [UC-003](../SDLC/01-srs.md#uc-003) Duyệt kế hoạch         | FR-002, FR-017, FR-025                 | NFR-001                         | —                                                                            |
| [UC-004](../SDLC/01-srs.md#uc-004) Quản lý lớp            | FR-006, FR-019, FR-020, FR-021, FR-024 | NFR-001, NFR-009                | —                                                                            |
| [UC-005](../SDLC/01-srs.md#uc-005) Cảnh báo bất thường    | FR-017, FR-021                         | NFR-001, NFR-011                | [AI-003](../SDLC/01-srs.md#ai-003)                                           |
| [UC-006](../SDLC/01-srs.md#uc-006) Đăng ký học phần       | FR-004, FR-007, FR-008, FR-019         | NFR-001, NFR-004, NFR-007       | —                                                                            |
| [UC-007](../SDLC/01-srs.md#uc-007) Xem học phí            | FR-009, FR-010                         | NFR-001                         | —                                                                            |
| [UC-008](../SDLC/01-srs.md#uc-008) Theo dõi học phí       | FR-009, FR-018, FR-023                 | NFR-001                         | [AI-008](../SDLC/01-srs.md#ai-008)                                           |
| [UC-009](../SDLC/01-srs.md#uc-009) Nhập điểm              | FR-011, FR-012, FR-017, FR-024         | NFR-001, NFR-009                | —                                                                            |
| [UC-010](../SDLC/01-srs.md#uc-010) Bảng điểm              | FR-013                                 | NFR-001                         | [AI-005](../SDLC/01-srs.md#ai-005)                                           |
| [UC-011](../SDLC/01-srs.md#uc-011) Lập lịch thi           | FR-014, FR-015, FR-019, FR-020, FR-022 | NFR-001                         | [AI-006](../SDLC/01-srs.md#ai-006)                                           |
| [UC-012](../SDLC/01-srs.md#uc-012) Phân phòng/giám thị    | FR-015, FR-020                         | NFR-001                         | —                                                                            |
| [UC-013](../SDLC/01-srs.md#uc-013) Khảo sát               | FR-016                                 | NFR-007, NFR-008                | —                                                                            |
| [UC-014](../SDLC/01-srs.md#uc-014) Xem đánh giá           | FR-016, FR-023                         | NFR-001, NFR-007                | [AI-007](../SDLC/01-srs.md#ai-007)                                           |
| [UC-015](../SDLC/01-srs.md#uc-015) Hỏi đáp học vụ         | FR-026                                 | NFR-002, NFR-015                | [AI-004](../SDLC/01-srs.md#ai-004)                                           |
| [UC-016](../SDLC/01-srs.md#uc-016) Dashboard lãnh đạo     | FR-002, FR-013, FR-023                 | NFR-001, NFR-007                | [AI-003](../SDLC/01-srs.md#ai-003), [AI-007](../SDLC/01-srs.md#ai-007)       |
| [UC-017](../SDLC/01-srs.md#uc-017) Quản lý CTĐT/học phần  | FR-004, FR-005, FR-024                 | NFR-001, NFR-007, NFR-009       | —                                                                            |
| [UC-018](../SDLC/01-srs.md#uc-018) Quản trị tài khoản     | FR-003, FR-024                         | NFR-007, NFR-009                | —                                                                            |

> Ghi chú phủ FR: FR-001 và FR-002 áp dụng cho **tất cả UC** (xác thực/phân quyền). Từ `01-srs.md` v0.5.0, các FR trước đây thiếu UC chuyên biệt (FR-003, FR-005) đã được phủ bởi UC-018 và UC-017; FR-025/FR-026 bổ sung phủ kế hoạch học tập và hỏi đáp học vụ.

---

## 6. Tham chiếu

- [`../SDLC/01-srs.md`](../SDLC/01-srs.md) — đặc tả yêu cầu (UC, FR, NFR, AI).
- [`GLOSSARY.md`](GLOSSARY.md) — định nghĩa thuật ngữ.
- [`PROJECT-STATE.md`](PROJECT-STATE.md) — trạng thái dự án & gap cần bổ sung.
- [`../SDLC/00-quy-chuan.md`](../SDLC/00-quy-chuan.md) — §8 truy ngược.
- [`../SDLC/10-architecture-decision-record.md`](../SDLC/10-architecture-decision-record.md) — ADR-002.
