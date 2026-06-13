# 00 — Quy chuẩn tài liệu

## 1. Thông tin tài liệu

### 1.1. Metadata

| Thuộc tính         | Giá trị                                                              |
| ------------------ | -------------------------------------------------------------------- |
| Tên tài liệu       | Quy chuẩn tài liệu                                                   |
| Mã tài liệu        | 00-quy-chuan                                                         |
| Dự án              | Nền tảng tích hợp hỗ trợ tổ chức đào tạo (NCKH_1)                    |
| Phiên bản          | v2.0.0                                                               |
| Trạng thái         | Draft                                                                |
| Người viết         | Hiếu, Khanh                                                          |
| Người duyệt        | Nguyễn Hồng Khanh                                                    |
| Ngày tạo           | 2026-05-26                                                           |
| Ngày cập nhật      | 2026-06-13                                                           |
| Tài liệu liên quan | [`../../CLAUDE.md`](../../CLAUDE.md), [`../agents.md`](../agents.md) |

### 1.2. Lịch sử thay đổi (Changelog)

| Phiên bản | Ngày       | Tác giả           | Thay đổi                                                                                                                                                                                                            |
| --------- | ---------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 2.0.0     | 2026-06-13 | Hiếu              | Rút gọn quy chuẩn; thêm §2 Chuẩn nền (ISO 15289 + 29148, tailoring, bảng từ khoá thống nhất); gộp mức độ nghĩa vụ vào §2; bỏ phần nguyên tắc thiết kế phần mềm; cập nhật vị trí glossary/traceability theo ADR-002. |
| 1.1.0     | 2026-05-26 | team-architecture | Thay frontmatter YAML bằng _bảng metadata hiển thị_; chuyển changelog lên đầu (sát chuẩn IEEE/ISO document control).                                                                                                |
| 1.0.0     | 2026-05-26 | team-architecture | Bản đầu tiên — chốt chuẩn tham chiếu, ID, RFC 2119, traceability, versioning, nguyên tắc thiết kế.                                                                                                                  |

---

Tài liệu này là **chuẩn cao nhất** về **hình thức & quy ước trình bày** cho toàn bộ thư mục `doc/` của dự án **NCKH_1**. Mọi tài liệu khác **BẮT BUỘC** tuân thủ. Khi xung đột:

- **Phạm vi & ranh giới dự án** → theo [`../../CLAUDE.md`](../../CLAUDE.md).
- **Thứ tự viết & nội dung tối thiểu mỗi tài liệu** → theo [`../agents.md`](../agents.md).
- **Hình thức & quy ước trình bày** → theo file này.

---

## 2. Chuẩn nền

Dự án dùng hai chuẩn cố định làm nền và không tự phát minh chuẩn mới:

| Mã chuẩn                | Vai trò trong dự án                                                            |
| ----------------------- | ------------------------------------------------------------------------------ |
| ISO/IEC/IEEE 15289:2019 | Xác định loại tài liệu, mục đích, đối tượng và nội dung bắt buộc của tài liệu. |
| ISO/IEC/IEEE 29148:2018 | Xác định cách viết, kiểm tra và quản lý yêu cầu hệ thống/phần mềm.             |

Các quy định riêng của dự án (mã tài liệu, danh mục file, trạng thái tài liệu, checklist) là phần **tailoring nội bộ**, không được mâu thuẫn với hai chuẩn nền trên.

KHÔNG ĐƯỢC ghi rằng dự án đã được chứng nhận ISO/IEC/IEEE nếu chưa có hoạt động đánh giá/chứng nhận độc lập.

Thuật ngữ bắt buộc dùng thống nhất trong toàn bộ tài liệu SDLC:

| Từ khóa       | Ý nghĩa                                                              |
| ------------- | -------------------------------------------------------------------- |
| BẮT BUỘC      | Phải tuân thủ. Nếu không tuân thủ thì tài liệu/code không đạt chuẩn. |
| KHÔNG ĐƯỢC    | Bị cấm, trừ khi có quyết định cập nhật tài liệu `00`.                |
| NÊN           | Khuyến nghị mạnh. Nếu không làm phải có lý do rõ ràng.               |
| CÓ THỂ        | Được phép áp dụng khi phù hợp, không bắt buộc.                       |
| TBD           | Thông tin chưa xác định.                                             |
| ASSUMPTION    | Giả định tạm thời, phải xác nhận sau.                                |
| OPEN QUESTION | Câu hỏi cần làm rõ trước khi duyệt hoặc triển khai.                  |
| RISK          | Rủi ro cần được theo dõi hoặc xử lý.                                 |
| DECISION      | Quyết định đã chốt.                                                  |

> **Đồng nghĩa & ánh xạ RFC 2119/8174:** _PHẢI_ ≡ **BẮT BUỘC** (MUST); _CẤM_ ≡ **KHÔNG ĐƯỢC** (MUST NOT); **NÊN** = SHOULD (_KHÔNG NÊN_ = SHOULD NOT khi cần); **CÓ THỂ** = MAY. Từ khoá viết hoa; văn bản không phải yêu cầu dùng từ thường ("hệ thống sẽ hiển thị ...").
>
> Các nhãn TBD / ASSUMPTION / OPEN QUESTION / RISK / DECISION đánh dấu trạng thái thông tin và **NÊN** kèm ID khi có (vd `RISK-004`, `ADR-002`).

> **Ký pháp/format áp dụng** (tailoring, không phải chuẩn nền): Mermaid (sơ đồ — §6), OpenAPI 3.1 (API), C4 Model (kiến trúc), ADR kiểu Michael Nygard (quyết định), STRIDE (mô hình mối đe doạ), Given-When-Then (acceptance). Nội dung tối thiểu của từng tài liệu: [`../agents.md`](../agents.md).

---

## 3. Quy ước file & thư mục

### 3.1. Đặt tên file

- File SDLC dùng tiền tố **2 chữ số + gạch ngang** (`01-srs.md`, `02-hld.md`, ...), zero-pad tới 99.
- Tên file dùng **kebab-case**, chữ thường, không dấu tiếng Việt.
- Hình ảnh/sơ đồ kèm theo đặt trong `assets/` cùng cấp; tên `<id-tài-liệu>-<mô-tả>.<ext>` (vd `02-hld-container-diagram.svg`).

### 3.2. Cấu trúc thư mục `doc/`

```
doc/
├── agents.md             ← chỉ dẫn cho agent
├── context/              ← bối cảnh, glossary, bản đồ miền, trạng thái dự án
└── SDLC/                 ← tài liệu vòng đời
    ├── 00-quy-chuan.md
    ├── 01-srs.md … 11-project-task-breakdown.md
    └── assets/           ← (tạo khi cần)
```

KHÔNG tạo thêm thư mục cấp 2 trong `SDLC/` trừ `assets/`. Khi cần phân mảnh file dài, dùng heading + ID thay vì tách file.

---

## 4. Cấu trúc chung của một tài liệu SDLC

Mọi tài liệu trong `SDLC/` **BẮT BUỘC** mở đầu bằng **§1 — Thông tin tài liệu** (bảng _Metadata_ + bảng _Lịch sử thay đổi_) và kết thúc bằng **§N — Tham chiếu**. Cấu trúc này hiện thực hoá yêu cầu nội dung tài liệu theo ISO/IEC/IEEE 15289.

### 4.1. Bảng Metadata (đầu file)

Dùng bảng Markdown (KHÔNG dùng YAML frontmatter) với các trường:

| Thuộc tính         | Bắt buộc | Ghi chú                                                         |
| ------------------ | -------- | --------------------------------------------------------------- |
| Tên tài liệu       | có       | Tên đầy đủ tiếng Việt (kèm tên Anh trong ngoặc nếu cần).        |
| Mã tài liệu        | có       | Trùng tên file không có `.md` (vd `01-srs`).                    |
| Dự án              | có       | "Nền tảng tích hợp hỗ trợ tổ chức đào tạo (NCKH_1)".            |
| Phiên bản          | có       | SemVer có chữ `v` (vd `v0.1.0`). Xem §9.                        |
| Trạng thái         | có       | `Draft` / `Review` / `Approved` / `Superseded`.                 |
| Người viết         | có       | Tác giả đóng góp chính.                                         |
| Người duyệt        | có       | Người phê duyệt khi `Approved`; lúc `Draft` ghi "(chưa duyệt)". |
| Ngày tạo           | có       | ISO 8601 `YYYY-MM-DD`.                                          |
| Ngày cập nhật      | có       | ISO 8601, cập nhật mỗi commit đổi nội dung.                     |
| Tài liệu liên quan | nên      | Link Markdown tới file phụ thuộc/được phụ thuộc.                |

### 4.2. Bảng Lịch sử thay đổi

Bảng 4 cột (`Phiên bản | Ngày | Tác giả | Thay đổi`), **dòng mới ở trên cùng**. Mỗi lần sửa **BẮT BUỘC** thêm 1 dòng và bump phiên bản (§9). Cột `Thay đổi` ≤ 200 ký tự, động từ chủ động.

### 4.3. Cấu trúc heading

- `#` (H1) duy nhất một lần — tiêu đề tài liệu.
- `##` (H2) cho phần lớn, đánh số `## 1.`, `## 2.`, ...
- `###` (H3) cho mục con `### 1.1.`, ...
- KHÔNG vượt quá H4; cần H5 thì tách mục.

### 4.4. Mục "Tham chiếu" (cuối file)

Mục cuối **BẮT BUỘC** là `## N. Tham chiếu`, liệt kê nguồn trích dẫn (tài liệu nội bộ + chuẩn ngoài), mỗi dòng kèm mô tả ngắn. Lịch sử thay đổi KHÔNG đặt ở cuối — đã ở §1.2.

---

## 5. Quy ước đặt ID

### 5.1. Tiền tố ID

| Loại                  | Tiền tố | Ví dụ      |
| --------------------- | ------- | ---------- |
| Use Case              | `UC`    | `UC-001`   |
| Yêu cầu chức năng     | `FR`    | `FR-007`   |
| Yêu cầu phi chức năng | `NFR`   | `NFR-002`  |
| Yêu cầu AI            | `AI`    | `AI-003`   |
| Quyết định kiến trúc  | `ADR`   | `ADR-012`  |
| Ca kiểm thử           | `TC`    | `TC-045`   |
| Rủi ro                | `RISK`  | `RISK-004` |
| Quy tắc nghiệp vụ     | `BR`    | `BR-009`   |
| Thực thể dữ liệu      | `ENT`   | `ENT-011`  |
| API endpoint          | `API`   | `API-023`  |
| Màn hình UI           | `UI`    | `UI-014`   |

### 5.2. Quy tắc đánh số

- Luôn **3 chữ số zero-pad** (`FR-007`, không `FR-7`).
- **Không tái sử dụng số**: khi bỏ một mục, chuyển trạng thái `deprecated`, giữ ID.
- **Không nhảy số có ý đồ**: đánh tuần tự; nhóm theo chủ đề bằng heading.

### 5.3. Trích dẫn ID

Ghi đầy đủ ID khi nhắc lần đầu trong một mục; khi cần link dùng anchor: `[`FR-007`](01-srs.md#fr-007)`.

---

## 6. Sơ đồ

- **Mặc định: Mermaid** (nhúng trong Markdown, diff được, render trên GitHub). Khi không đủ (wireframe), dùng ảnh `.svg` trong `assets/`.
- Mỗi sơ đồ kèm **1 đoạn diễn giải**; sơ đồ không thay thế văn bản.
- Đặt tên node bằng định danh ASCII; nhãn hiển thị trong `[]`/`()` có thể dùng tiếng Việt. Hướng đọc mặc định `LR` (flowchart) hoặc `TB` (phân cấp).

| Mục đích                        | Loại Mermaid            | Áp dụng                                |
| ------------------------------- | ----------------------- | -------------------------------------- |
| Bối cảnh / Container (C4 L1–L2) | `flowchart`             | `02-hld.md`                            |
| Component (C4 L3)               | `flowchart`             | `03-lld.md`                            |
| Lớp / thực thể miền             | `classDiagram`          | `03-lld.md`                            |
| Cơ sở dữ liệu                   | `erDiagram`             | `04-database-design.md`                |
| Tương tác theo thời gian        | `sequenceDiagram`       | `03-lld.md`, `05-api-specification.md` |
| Vòng đời thực thể               | `stateDiagram-v2`       | `03-lld.md`                            |
| Luồng / hành trình người dùng   | `flowchart` / `journey` | `06-ui-ux-flow-specification.md`       |
| Lịch trình                      | `gantt`                 | `11-project-task-breakdown.md`         |

---

## 7. Bảng, danh sách & liên kết chéo

- **Bảng** dùng khi ≥ 3 cột HOẶC ≥ 4 hàng cấu trúc lặp lại; **BẮT BUỘC** có hàng tiêu đề và dấu căn lề (`:---`, `:---:`, `---:`).
- **Bullet list** cho liệt kê ngắn (≤ 7 mục); **numbered list** khi thứ tự có nghĩa.
- **Mọi tham chiếu chéo BẮT BUỘC là link Markdown** tới anchor cụ thể (không "xem tài liệu 03"); link tương đối, dùng `/`.
- Anchor sinh tự động từ heading. Để ổn định khi sửa câu chữ, thêm anchor cố định ngay sau heading:

  ```markdown
  ### FR-007 — Gợi ý môn học theo chương trình

  <a id="fr-007"></a>
  ```

---

## 8. Truy ngược (traceability)

Toàn dự án **BẮT BUỘC** giữ một chuỗi truy ngược tối thiểu:

```
Nhu cầu nghiệp vụ (CLAUDE.md §1)
  └─► Use Case (UC-XXX)
        └─► Yêu cầu chức năng (FR-XXX) / phi chức năng (NFR-XXX) / AI (AI-XXX)
              ├─► Quyết định kiến trúc (ADR-XXX)
              ├─► Component / API / Bảng CSDL
              └─► Ca kiểm thử (TC-XXX)
```

- Ma trận truy ngược được duy trì **tập trung tại một nơi** (vị trí hiện hành: [`../context/DOMAIN-MAP.md`](../context/DOMAIN-MAP.md) — xem `ADR-002`).
- Khi thêm/xoá một mục trong chuỗi trên, **BẮT BUỘC** cập nhật ma trận trong **cùng commit**.
- `08-test-plan-acceptance-criteria.md` chịu trách nhiệm phụ: mỗi `TC` chỉ rõ phủ FR/UC nào.

---

## 9. Quản lý phiên bản & trạng thái

### 9.1. Phiên bản (SemVer mức tài liệu)

- `MAJOR` — phá vỡ cấu trúc tài liệu hoặc đổi quyết định cốt lõi.
- `MINOR` — thêm/bớt yêu cầu, mục mới.
- `PATCH` — sửa chính tả, làm rõ, không đổi nghĩa.

Mỗi lần sửa **BẮT BUỘC** cập nhật `Phiên bản` + `Ngày cập nhật` trong Metadata và thêm dòng vào Lịch sử thay đổi.

### 9.2. Trạng thái & review

| Trạng thái   | Ý nghĩa                                                   |
| ------------ | --------------------------------------------------------- |
| `Draft`      | Đang viết, có thể thay đổi lớn.                           |
| `Review`     | Sẵn sàng duyệt; không tự ý sửa nữa.                       |
| `Approved`   | Đã được người duyệt phê duyệt.                            |
| `Superseded` | Đã có bản thay thế — ghi rõ tài liệu thay thế ở đầu file. |

Quy trình: tác giả đặt `Review` + mở pull request → ≥ 1 _Người duyệt_ approve → khi merge đổi sang `Approved`, ghi tên người duyệt, bump phiên bản, cập nhật ngày.

---

## 10. Quy ước nội dung

- **Ngôn ngữ:** tiếng Việt là chính, câu ngắn, chủ ngữ rõ. Thuật ngữ tiếng Anh giữ nguyên khi không có bản dịch chuẩn — _in nghiêng_ lần đầu kèm chú thích: _use case_ (ca sử dụng). Định danh kỹ thuật (biến, bảng, API, header) **luôn dùng tiếng Anh**.
- **Giọng văn:** chủ động > bị động ("Hệ thống ghi log"); tránh sáo ngữ ("phù hợp", "linh hoạt", "tối ưu") — thay bằng con số; mỗi câu một ý.
- **Đo lường:** số có đơn vị ("≤ 300 ms ở p95"); dùng phân vị (p50/p95/p99) thay trung bình cho hiệu năng; ngưỡng AI **BẮT BUỘC** kèm bộ dữ liệu đánh giá, cách đo, ngưỡng đậu.

---

## 11. Quy ước thuật ngữ

- Thuật ngữ dùng chung được thống nhất tại **một nguồn duy nhất**: [`../context/GLOSSARY.md`](../context/GLOSSARY.md) (xem `ADR-002`). Các tài liệu khác **liên kết** về đó, **KHÔNG nhân bản** định nghĩa.
- Định nghĩa theo cấu trúc: `**Thuật ngữ** (_English_) — giải thích ngắn, phân biệt với khái niệm gần.`

---

## 12. Tham chiếu

- [`../../CLAUDE.md`](../../CLAUDE.md) — phạm vi & ranh giới dự án.
- [`../agents.md`](../agents.md) — thứ tự viết & nội dung tối thiểu mỗi tài liệu.
- [`../context/GLOSSARY.md`](../context/GLOSSARY.md), [`../context/DOMAIN-MAP.md`](../context/DOMAIN-MAP.md) — glossary & ma trận truy ngược.
- [`10-architecture-decision-record.md`](10-architecture-decision-record.md) — ADR-002 (vị trí glossary & traceability).
- ISO/IEC/IEEE 15289:2019 — Content of life-cycle information items (documentation).
- ISO/IEC/IEEE 29148:2018 — Requirements engineering.
- RFC 2119 / RFC 8174 — Key words to Indicate Requirement Levels.
