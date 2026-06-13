# PROJECT-STATE — Trạng thái dự án NCKH_1

## 1. Thông tin tài liệu

### 1.1. Metadata

| Thuộc tính         | Giá trị                                                                                                   |
| :----------------- | :-------------------------------------------------------------------------------------------------------- |
| Tên tài liệu       | Trạng thái dự án (_Project state_)                                                                        |
| Mã tài liệu        | PROJECT-STATE                                                                                              |
| Dự án              | Nền tảng tích hợp hỗ trợ tổ chức đào tạo (NCKH_1)                                                          |
| Phiên bản          | v0.3.0                                                                                                     |
| Trạng thái         | Draft (tài liệu sống — cập nhật mỗi cuối phase)                                                           |
| Người viết         | Hiếu                                                                                                       |
| Người duyệt        | (chưa duyệt)                                                                                               |
| Ngày tạo           | 2026-06-13                                                                                                 |
| Ngày cập nhật      | 2026-06-13                                                                                                 |
| Tài liệu liên quan | [`../../CLAUDE.md`](../../CLAUDE.md), [`../SDLC/01-srs.md`](../SDLC/01-srs.md), [`01-muc-tieu-nghien-cuu-ai.md`](01-muc-tieu-nghien-cuu-ai.md), [`02-ke-hoach-chuan-bi-nghien-cuu.md`](02-ke-hoach-chuan-bi-nghien-cuu.md), [`DOMAIN-MAP.md`](DOMAIN-MAP.md) |

### 1.2. Lịch sử thay đổi (Changelog)

| Phiên bản | Ngày       | Tác giả  | Thay đổi                                                                                  |
| :-------- | :--------- | :------- | :---------------------------------------------------------------------------------------- |
| 0.3.0     | 2026-06-13 | AI Agent | Cập nhật trạng thái sau khi dựng skeleton 02-09 và 11; giữ SRS gap/open questions hiện hành. |
| 0.2.0     | 2026-06-13 | AI Agent | Cập nhật trạng thái sau khi `01-srs.md` v0.5.0 xử lý các gap UC/FR và ghi Open Questions. |
| 0.1.0     | 2026-06-13 | Hiếu     | Tạo tài liệu trạng thái: giai đoạn hiện tại, trạng thái tài liệu, quyết định mở, gap & bước kế tiếp. |

---

Tài liệu **sống** ghi nhận dự án đang ở đâu trong vòng đời. Cập nhật **mỗi khi qua một cột mốc** (đổi trạng thái tài liệu, đóng một quyết định, hoàn thành một phase). Mục tiêu: bất kỳ ai (người hoặc agent) mở dự án đều biết ngay "đang ở đâu, còn vướng gì".

---

## 2. Giai đoạn hiện tại

**Phase 0 — Foundation** (theo [`02-ke-hoach-chuan-bi-nghien-cuu.md §4.3`](02-ke-hoach-chuan-bi-nghien-cuu.md#43-phase-0--foundation)). Đang soạn tài liệu nền; **chưa hoàn thành** cột mốc ra Phase 0.

| Cột mốc ra Phase 0                 | Trạng thái     |
| :--------------------------------- | :------------- |
| SRS ở `Review`                     | ⏳ đang Draft  |
| HLD ở `Review`                     | ❌ chưa bắt đầu |
| ADR-001 (cặp LLM provider) `Approved` | ⏳ Proposed   |
| Repo có CI xanh                    | ❌ chưa         |
| 2 API key + budget alert           | ❌ chưa         |

---

## 3. Trạng thái tài liệu

### 3.1. SDLC (`doc/SDLC/`)

| Tài liệu                              | Trạng thái   | Ghi chú                                              |
| :------------------------------------ | :----------- | :--------------------------------------------------- |
| `00-quy-chuan.md`                     | Draft v2.0.0 | Rút gọn; thêm §2 Chuẩn nền; dùng làm gốc.            |
| `01-srs.md`                           | Draft v0.5.0 | Bổ sung FR-025/026, UC-017/018; glossary & traceability đã tách ra context; còn Open Questions trước khi lên Review. |
| `02-hld.md` … `09-…`                  | Skeleton v0.1.0 | Đã dựng khung ban đầu; cần bổ sung nội dung.        |
| `10-architecture-decision-record.md`  | Draft v0.1.0 | ADR-001 (Proposed), ADR-002 (Accepted).             |
| `11-project-task-breakdown.md`        | Skeleton v0.1.0 | Đã dựng khung ban đầu; cần bổ sung WBS.             |

### 3.2. Context (`doc/context/`)

| Tài liệu                              | Trạng thái   | Nội dung                                             |
| :------------------------------------ | :----------- | :--------------------------------------------------- |
| `01-muc-tieu-nghien-cuu-ai.md`        | Draft v0.1.0 | Hướng nghiên cứu C; khung α/β/γ; M1–M6; H1–H3.       |
| `02-ke-hoach-chuan-bi-nghien-cuu.md`  | Draft v0.1.0 | 5 phần chuẩn bị; lộ trình 6 phase.                   |
| `GLOSSARY.md`                         | Draft v0.1.0 | Thuật ngữ dùng chung (tách khỏi SRS theo ADR-002).  |
| `DOMAIN-MAP.md`                       | Draft v0.2.0 | Bản đồ miền + ma trận truy ngược, đã đồng bộ SRS.   |
| `PROJECT-STATE.md`                    | Draft v0.1.0 | Tài liệu này.                                        |

---

## 4. Hướng nghiên cứu

Hướng **C — Pure LLM end-to-end** cho sinh thời khoá biểu (γ kèm verifier, không solver trong luồng chính). So sánh 3 hệ α/β/γ trên VACS theo M1–M6; kiểm định H1–H3. Chi tiết: [`01-muc-tieu-nghien-cuu-ai.md`](01-muc-tieu-nghien-cuu-ai.md). Các năng lực AI khác theo lối "AI thực dụng" (LLM + RAG + người duyệt).

---

## 5. Quyết định đang mở

| Mã        | Quyết định                                                              | Hạn / điều kiện         | Trạng thái |
| :-------- | :---------------------------------------------------------------------- | :---------------------- | :--------- |
| ADR-001   | Cặp LLM provider (flagship + efficient, ≥ 2 nhà).                       | Trước Phase 2           | Proposed   |
| D-API-05  | Budget token tổng cho toàn dự án.                                       | Trước Phase 1           | Mở         |
| ORDER-01  | Thứ tự dataset ↔ α (vòng phụ thuộc `gold_solutions`).                  | Trước Phase 1           | Mở         |
| DATA-01   | Nguồn dữ liệu thật của khoa/trường (quy chế, CTĐT, lớp).               | Phase 0 (xin sớm)       | Mở (RISK-102) |

---

## 6. Gap nội dung & bước kế tiếp

### 6.1. Gap trong `01-srs.md`

Các gap yêu cầu bắt buộc đã được xử lý trong [`01-srs.md` v0.5.0](../SDLC/01-srs.md):

- FR cho vòng đời kế hoạch học tập: bổ sung `FR-025`, ánh xạ `UC-001` và `UC-003`.
- FR cho giao diện hỏi đáp học vụ: bổ sung `FR-026`, ánh xạ `UC-015` và `AI-004`.
- UC cho Khoa quản lý chương trình/học phần: bổ sung `UC-017`, ánh xạ `FR-004`, `FR-005`, `FR-024`.
- UC cho SysAdmin quản trị tài khoản: bổ sung `UC-018`, ánh xạ `FR-003`, `FR-024`.

Còn 4 câu hỏi mở cần chốt trước khi đưa SRS lên `Review`: `OPEN-001` nguồn dữ liệu thật, `OPEN-002` LLM provider/budget, `OPEN-003` chính sách duyệt kế hoạch trước đăng ký, `OPEN-004` ngưỡng nghiệp vụ chính thức.

### 6.2. Bước kế tiếp đề xuất

1. Chốt các Open Questions trong [`01-srs.md §18`](../SDLC/01-srs.md#18-open-questions), đặc biệt `OPEN-001`, `OPEN-002`, `OPEN-003`.
2. Điền nội dung chi tiết cho `02-hld.md` → `11-project-task-breakdown.md` theo thứ tự phụ thuộc.
3. Rà lần cuối SRS + `DOMAIN-MAP.md`; nếu không còn mâu thuẫn, đổi SRS sang `Review`.
4. Chốt ADR-001 (cặp LLM provider) + budget API.
5. Setup repo CI/lint/test; đăng ký API key.

---

## 7. Tham chiếu

- [`../../CLAUDE.md`](../../CLAUDE.md) — phạm vi & ranh giới dự án.
- [`01-muc-tieu-nghien-cuu-ai.md`](01-muc-tieu-nghien-cuu-ai.md) — hướng nghiên cứu AI.
- [`02-ke-hoach-chuan-bi-nghien-cuu.md`](02-ke-hoach-chuan-bi-nghien-cuu.md) — lộ trình 6 phase.
- [`../SDLC/01-srs.md`](../SDLC/01-srs.md) — đặc tả yêu cầu phần mềm hiện tại.
- [`DOMAIN-MAP.md`](DOMAIN-MAP.md) — bản đồ miền & truy ngược.
- [`../SDLC/10-architecture-decision-record.md`](../SDLC/10-architecture-decision-record.md) — các quyết định kiến trúc.
