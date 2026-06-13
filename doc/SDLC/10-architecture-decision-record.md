# 10 — Nhật ký Quyết định Kiến trúc (Architecture Decision Record)

## 1. Thông tin tài liệu

### 1.1. Metadata

| Thuộc tính         | Giá trị                                                                                                   |
| :----------------- | :-------------------------------------------------------------------------------------------------------- |
| Tên tài liệu       | Nhật ký Quyết định Kiến trúc (_Architecture Decision Record_)                                             |
| Mã tài liệu        | 10-architecture-decision-record                                                                           |
| Dự án              | Nền tảng tích hợp hỗ trợ tổ chức đào tạo (NCKH_1)                                                          |
| Phiên bản          | v0.1.0                                                                                                     |
| Trạng thái         | Draft                                                                                                      |
| Người viết         | Hiếu                                                                                                       |
| Người duyệt        | (chưa duyệt)                                                                                               |
| Ngày tạo           | 2026-06-13                                                                                                 |
| Ngày cập nhật      | 2026-06-13                                                                                                 |
| Tài liệu liên quan | [`../../CLAUDE.md`](../../CLAUDE.md), [`00-quy-chuan.md`](00-quy-chuan.md), [`01-srs.md`](01-srs.md), [`../context/01-muc-tieu-nghien-cuu-ai.md`](../context/01-muc-tieu-nghien-cuu-ai.md) |

### 1.2. Lịch sử thay đổi (Changelog)

| Phiên bản | Ngày       | Tác giả | Thay đổi                                                                            |
| :-------- | :--------- | :------ | :---------------------------------------------------------------------------------- |
| 0.1.0     | 2026-06-13 | Hiếu    | Tạo nhật ký ADR; ADR-001 (Proposed — cặp LLM provider), ADR-002 (Accepted — tách glossary & traceability). |

---

Mỗi quyết định là một mục theo format Michael Nygard: `Status`, `Context`, `Decision`, `Consequences`. Không tái sử dụng số ADR; khi một ADR bị thay thế, đặt `Status: Superseded by ADR-XXX`.

---

## 2. ADR-001 — Lựa chọn cặp LLM provider

<a id="adr-001"></a>

**Status:** Proposed (chưa quyết — dự kiến chốt trước Phase 2, xem [`../context/02-ke-hoach-chuan-bi-nghien-cuu.md §3.3`](../context/02-ke-hoach-chuan-bi-nghien-cuu.md#33-api-llm-backbone)).

**Context:** Hướng nghiên cứu C cần LLM backbone qua API (không pre-train). Yêu cầu ≥ 2 nhà cung cấp để loại trừ phụ thuộc một mô hình và giảm rủi ro vendor lock-in. Hai cặp ứng viên: P1 (Claude Opus + Haiku) và P2 (GPT-4o + GPT-4o-mini); hoặc cặp xuyên nhà cung cấp.

**Decision:** _Chưa chốt._ Sẽ quyết khi hoàn thiện SRS/HLD, ghi lại tại mục này.

**Consequences:** Khi chốt sẽ ảnh hưởng: wrapper client, budget token, chiến lược caching. Giữ trạng thái Proposed để không chặn các tài liệu khác.

---

## 3. ADR-002 — Tách Glossary & Ma trận truy ngược ra `doc/context/`

<a id="adr-002"></a>

**Status:** Accepted (2026-06-13).

**Context:** [`01-srs.md`](01-srs.md) chứa Glossary (§18) và Ma trận truy ngược (§19). Hai phần này mang tính **dùng chung xuyên tài liệu**: glossary được mọi tài liệu SDLC tham chiếu; ma trận truy ngược liên kết UC/FR/NFR/AI và sẽ được nhiều tài liệu cập nhật. Giữ chúng trong SRS gây nhân bản và làm SRS phình to. [`00-quy-chuan.md §10`](00-quy-chuan.md#10-truy-ngược-traceability) quy định ma trận truy ngược "duy trì ở phần cuối của `01-srs.md`", và [`§14`](00-quy-chuan.md#14-quy-ước-thuật-ngữ-glossary) quy định glossary thống nhất trong `01-srs.md §glossary`.

**Decision:**

- Chuyển Glossary sang [`../context/GLOSSARY.md`](../context/GLOSSARY.md) làm **nguồn sự thật duy nhất** cho thuật ngữ; mọi tài liệu liên kết về đây.
- Chuyển Ma trận truy ngược sang [`../context/DOMAIN-MAP.md`](../context/DOMAIN-MAP.md) làm bản duy trì chính thức.
- **Giữ** mục `Tham chiếu` trong `01-srs.md` (đổi số thành §18) — không vi phạm [`00-quy-chuan.md §4.4`](00-quy-chuan.md#44-mục-tham-chiếu-cuối-tài-liệu-bắt-buộc).
- Cập nhật [`../agents.md`](../agents.md) để chỉ rõ vị trí mới của glossary & traceability.

**Consequences:**

- _Tích cực:_ một nguồn glossary/traceability dùng chung; SRS gọn hơn; tránh hai bản lệch nhau.
- _Tiêu cực (chấp nhận):_ lệch `00-quy-chuan §10` và `§14` về _vị trí_. Giảm thiểu bằng: (a) liên kết hai chiều rõ ràng giữa SRS ↔ context; (b) `agents.md` chỉ dẫn vị trí mới; (c) ADR này là bản ghi ngoại lệ.
- _Điều kiện quay lại tuân thủ:_ nếu sau này quyết gộp lại vào SRS, đưa nội dung trở về §18/§19 và đặt `Status: Superseded`.

---

## 4. Tham chiếu

- [`00-quy-chuan.md`](00-quy-chuan.md) — §10 truy ngược, §14 glossary, §13.2 quy trình khi vi phạm nguyên tắc.
- [`01-srs.md`](01-srs.md) — tài liệu chịu tác động của ADR-002.
- [`../context/GLOSSARY.md`](../context/GLOSSARY.md), [`../context/DOMAIN-MAP.md`](../context/DOMAIN-MAP.md) — vị trí mới.
- Michael Nygard, "Documenting Architecture Decisions" (2011).
