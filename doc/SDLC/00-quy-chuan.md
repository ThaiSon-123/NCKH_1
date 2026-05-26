# 00 — Quy chuẩn tài liệu & xây dựng phần mềm

## 1. Thông tin tài liệu

### 1.1. Metadata

| Thuộc tính         | Giá trị                                            |
| ------------------ | -------------------------------------------------- |
| Tên tài liệu       | Quy chuẩn tài liệu & xây dựng phần mềm             |
| Mã tài liệu        | 00-quy-chuan                                       |
| Dự án              | Nền tảng tích hợp hỗ trợ tổ chức đào tạo (NCKH_1)  |
| Phiên bản          | v1.1.0                                             |
| Trạng thái         | Draft                                              |
| Người viết         | team-architecture                                  |
| Người duyệt        | (chưa duyệt)                                       |
| Ngày tạo           | 2026-05-26                                         |
| Ngày cập nhật      | 2026-05-26                                         |
| Tài liệu liên quan | [`../../CLAUDE.md`](../../CLAUDE.md), [`../agents.md`](../agents.md) |

### 1.2. Lịch sử thay đổi (Changelog)

| Phiên bản | Ngày       | Tác giả           | Thay đổi                                                                                                       |
| --------- | ---------- | ----------------- | -------------------------------------------------------------------------------------------------------------- |
| 1.1.0     | 2026-05-26 | team-architecture | Thay frontmatter YAML bằng *bảng metadata hiển thị*; chuyển changelog lên đầu (sát chuẩn IEEE/ISO document control). |
| 1.0.0     | 2026-05-26 | team-architecture | Bản đầu tiên — chốt chuẩn tham chiếu, ID, RFC 2119, traceability, versioning, nguyên tắc thiết kế.            |

---

Tài liệu này là **chuẩn cao nhất** áp dụng cho toàn bộ thư mục `doc/` của dự án
**NCKH_1**. Mọi tài liệu khác (`01-srs.md` ... `11-project-task-breakdown.md`,
và các tài liệu phát sinh) **PHẢI** tuân thủ. Khi có xung đột giữa quy chuẩn
này và hướng dẫn cụ thể của một file, áp dụng nguyên tắc:

- **Phạm vi & ranh giới dự án** → theo [`../../CLAUDE.md`](../../CLAUDE.md).
- **Thứ tự viết & nội dung tối thiểu mỗi tài liệu** → theo
  [`../agents.md`](../agents.md).
- **Hình thức & quy ước trình bày** → theo file này.

---

## 2. Chuẩn tham chiếu

Dự án không phát minh lại bánh xe. Khi viết tài liệu, hãy **mượn cấu trúc của
các chuẩn quốc tế** sau, **không** sao chép nguyên văn:

| Chủ đề               | Chuẩn áp dụng                                       | Áp dụng cho                           |
| -------------------- | --------------------------------------------------- | ------------------------------------- |
| Kỹ nghệ yêu cầu      | ISO/IEC/IEEE 29148:2018                             | `01-srs.md`                           |
| Mô tả kiến trúc      | ISO/IEC/IEEE 42010:2022 + C4 Model                  | `02-hld.md`, `03-lld.md`              |
| Thiết kế phần mềm    | IEEE 1016-2009                                      | `03-lld.md`                           |
| API HTTP             | OpenAPI 3.1                                         | `05-api-specification.md`             |
| Quyết định kiến trúc | Michael Nygard ADR                                  | `10-architecture-decision-record.md`  |
| Tài liệu kiểm thử    | ISO/IEC/IEEE 29119 (phần 3)                         | `08-test-plan-acceptance-criteria.md` |
| Bảo mật thông tin    | ISO/IEC 27001 (kiểm soát liên quan) + OWASP ASVS L2 | `07-security-permission-design.md`    |
| Mô hình mối đe doạ   | STRIDE (Microsoft)                                  | `07-security-permission-design.md`    |
| Vận hành             | Google SRE workbook (chương về SLO/runbook)         | `09-deployment-operation-standard.md` |
| Phân chia công việc  | PMBOK 7 — Work Breakdown Structure                  | `11-project-task-breakdown.md`        |
| Truy ngược yêu cầu   | ISO/IEC/IEEE 29148:2018 §9 — Traceability           | xuyên suốt                            |
| Mức độ nghĩa vụ      | RFC 2119 + RFC 8174                                 | xuyên suốt                            |

> Mục đích: người mới vào dự án đọc một tài liệu là **nhận ra ngay cấu trúc**.
> Không phải để khoe có tham chiếu chuẩn.

---

## 3. Quy ước file & thư mục

### 3.1. Đặt tên file

- File SDLC dùng tiền tố **2 chữ số + dấu gạch ngang** (`01-srs.md`,
  `02-hld.md`, ...). **PHẢI** zero-pad cho tới 99.
- Tên file dùng **kebab-case**, toàn bộ chữ thường, không dấu tiếng Việt.
- Hình ảnh / sơ đồ kèm theo đặt trong thư mục con `assets/` cùng cấp với file
  tham chiếu. Tên: `<id-tài-liệu>-<mô-tả>.<ext>` — ví dụ
  `02-hld-container-diagram.svg`.

### 3.2. Cấu trúc thư mục `doc/`

```
doc/
├── agents.md             ← chỉ dẫn cho agent
├── context/              ← bối cảnh: phỏng vấn, ghi chú, biên bản
└── SDLC/                 ← tài liệu vòng đời
    ├── 00-quy-chuan.md
    ├── 01-srs.md
    ├── ...
    └── assets/           ← (tạo khi cần)
```

**KHÔNG** tạo thêm thư mục cấp 2 trong `SDLC/` trừ `assets/`. Khi cần phân
mảnh một file dài, dùng heading + ID thay vì tách file.

---

## 4. Cấu trúc chung của một tài liệu SDLC

Mọi tài liệu trong `SDLC/` **PHẢI** mở đầu bằng mục **§1 — Thông tin tài liệu**
(gồm bảng *Metadata* và bảng *Lịch sử thay đổi*) và **PHẢI** kết thúc bằng mục
**§N — Tham chiếu**. File này (`00-quy-chuan.md`) tuân thủ chính quy ước đó —
xem [`§1`](#1-thông-tin-tài-liệu) ở trên làm ví dụ.

### 4.1. Bảng thông tin tài liệu (đầu file, bắt buộc)

Mục §1.1 *Metadata* dùng bảng Markdown với các trường cố định dưới đây.
**Không** dùng YAML frontmatter — bảng hiển thị có lợi cho người đọc và
là chuẩn *document control* theo ISO/IEC/IEEE 29148.

| Thuộc tính         | Bắt buộc | Ghi chú                                                                               |
| ------------------ | -------- | ------------------------------------------------------------------------------------- |
| Tên tài liệu       | có       | Tên đầy đủ tiếng Việt (có thể kèm tên Anh trong ngoặc).                               |
| Mã tài liệu        | có       | Trùng tên file không có `.md` — ví dụ `01-srs`, `02-hld`.                             |
| Dự án              | có       | "Nền tảng tích hợp hỗ trợ tổ chức đào tạo (NCKH_1)".                                  |
| Phiên bản          | có       | Semver, có chữ `v` — ví dụ `v0.1.0`. Xem §11.                                         |
| Trạng thái         | có       | Một trong: `Draft`, `Review`, `Approved`, `Superseded`.                               |
| Người viết         | có       | Họ tên (hoặc tên đội) tác giả đóng góp chính.                                         |
| Người duyệt        | có       | Người phê duyệt khi `Trạng thái = Approved`. Lúc `Draft` ghi "(chưa duyệt)".          |
| Ngày tạo           | có       | ISO 8601 `YYYY-MM-DD`.                                                                |
| Ngày cập nhật      | có       | ISO 8601, cập nhật mỗi commit thay đổi nội dung.                                      |
| Tài liệu liên quan | nên      | Link Markdown tới các file phụ thuộc/được phụ thuộc.                                  |

### 4.2. Bảng lịch sử thay đổi (đầu file, bắt buộc)

Mục §1.2 *Lịch sử thay đổi* dùng bảng Markdown 4 cột, **dòng mới ở trên cùng**
(phiên bản mới nhất xuất hiện trước):

```markdown
| Phiên bản | Ngày       | Tác giả     | Thay đổi           |
| --------- | ---------- | ----------- | ------------------ |
| 0.1.0     | 2026-05-26 | Tên/Vai trò | Bản nháp đầu tiên. |
```

- Mỗi lần sửa **PHẢI** thêm 1 dòng và **PHẢI** bump `Phiên bản` theo §11.
- Cột `Thay đổi` mô tả ngắn gọn (≤ 200 ký tự) bằng động từ chủ động:
  "thêm UC-007", "đổi ngưỡng p95 lên 500 ms", v.v.

### 4.3. Cấu trúc heading

- `#` (H1) **duy nhất một lần** ở đầu nội dung, là tiêu đề tài liệu.
- `##` (H2) cho mỗi phần lớn — đánh số `## 1.`, `## 2.`, ...
- `###` (H3) cho mục con — đánh số `### 1.1.`, `### 1.2.`, ...
- **Không vượt quá H4.** Nếu thấy cần H5, hãy tách mục.

### 4.4. Mục "Tham chiếu" (cuối tài liệu, bắt buộc)

Mục cuối cùng của tài liệu **PHẢI** là `## N. Tham chiếu`, liệt kê các nguồn
được trích dẫn (tài liệu nội bộ + chuẩn ngoài), mỗi dòng kèm mô tả ngắn:

```markdown
## N. Tham chiếu

- [Tên tài liệu](đường-dẫn) — mô tả ngắn.
- ISO/IEC/IEEE 29148:2018 — Requirements engineering.
```

> Lịch sử thay đổi **không** đặt ở cuối — đã được đưa lên §1.2 theo chuẩn
> *document control*.

---

## 5. Quy ước đặt ID

### 5.1. Tiền tố ID theo loại

| Loại                  | Tiền tố | Định dạng  | Ví dụ      |
| --------------------- | ------- | ---------- | ---------- |
| Use Case              | `UC`    | `UC-XXX`   | `UC-001`   |
| Yêu cầu chức năng     | `FR`    | `FR-XXX`   | `FR-007`   |
| Yêu cầu phi chức năng | `NFR`   | `NFR-XXX`  | `NFR-002`  |
| Yêu cầu AI            | `AI`    | `AI-XXX`   | `AI-003`   |
| Quyết định kiến trúc  | `ADR`   | `ADR-XXX`  | `ADR-012`  |
| Ca kiểm thử           | `TC`    | `TC-XXX`   | `TC-045`   |
| Rủi ro                | `RISK`  | `RISK-XXX` | `RISK-004` |
| Quy tắc nghiệp vụ     | `BR`    | `BR-XXX`   | `BR-009`   |
| Thực thể dữ liệu      | `ENT`   | `ENT-XXX`  | `ENT-011`  |
| API endpoint          | `API`   | `API-XXX`  | `API-023`  |
| Màn hình UI           | `UI`    | `UI-XXX`   | `UI-014`   |

### 5.2. Quy tắc đánh số

- **Luôn 3 chữ số zero-pad** (`FR-007`, không phải `FR-7`).
- **Không tái sử dụng số.** Khi xoá một yêu cầu, chuyển trạng thái sang
  `deprecated` (giữ ID), không giải phóng số.
- **Không nhảy số có ý đồ.** Đánh tuần tự. Nhóm theo chủ đề dùng heading,
  không dùng dải số.

### 5.3. Trích dẫn ID

Trong văn bản, **PHẢI** ghi đầy đủ ID khi nhắc lần đầu trong một mục:

> Yêu cầu `FR-007` mô tả việc gợi ý môn học theo chương trình ...

Khi cần link, dùng anchor:

```markdown
[`FR-007`](01-srs.md#fr-007-goi-y-mon-hoc)
```

---

## 6. Mức độ nghĩa vụ (RFC 2119 / RFC 8174)

Khi phát biểu yêu cầu, dùng từ khoá **viết hoa**:

| Từ khoá                  | Nghĩa           | Ghi chú                                      |
| ------------------------ | --------------- | -------------------------------------------- |
| **PHẢI** / **BẮT BUỘC**  | MUST / REQUIRED | Không có ngoại lệ.                           |
| **KHÔNG ĐƯỢC** / **CẤM** | MUST NOT        | Cấm tuyệt đối.                               |
| **NÊN**                  | SHOULD          | Có thể bỏ qua với lý do hợp lý, ghi vào ADR. |
| **KHÔNG NÊN**            | SHOULD NOT      | Tương tự, kèm lý do.                         |
| **CÓ THỂ**               | MAY / OPTIONAL  | Tuỳ chọn.                                    |

Văn bản không phải yêu cầu (giải thích, ví dụ) dùng từ thường: "hệ thống sẽ
hiển thị ...", "có khả năng ...".

> Quy ước này áp dụng đặc biệt nghiêm ngặt cho `01-srs.md` và
> `07-security-permission-design.md`.

---

## 7. Sơ đồ (diagram)

### 7.1. Công cụ

- **Ngôn ngữ mặc định: Mermaid.** Lý do: nhúng trực tiếp trong Markdown,
  diff được, render trên GitHub.
- Khi Mermaid không đủ (sơ đồ bố cục UI, wireframe), dùng file ảnh (`.svg`
  ưu tiên hơn `.png`) trong thư mục `assets/`.

### 7.2. Loại sơ đồ theo mục đích

| Mục đích                     | Loại Mermaid      | Áp dụng                                |
| ---------------------------- | ----------------- | -------------------------------------- |
| Bối cảnh hệ thống (C4 L1)    | `flowchart`       | `02-hld.md`                            |
| Container (C4 L2)            | `flowchart`       | `02-hld.md`                            |
| Component (C4 L3)            | `flowchart`       | `03-lld.md`                            |
| Lớp / thực thể miền          | `classDiagram`    | `03-lld.md`                            |
| Cơ sở dữ liệu                | `erDiagram`       | `04-database-design.md`                |
| Tương tác theo thời gian     | `sequenceDiagram` | `03-lld.md`, `05-api-specification.md` |
| Vòng đời thực thể            | `stateDiagram-v2` | `03-lld.md`                            |
| Luồng người dùng / nghiệp vụ | `flowchart`       | `06-ui-ux-flow-specification.md`       |
| Hành trình người dùng        | `journey`         | `06-ui-ux-flow-specification.md`       |
| Lịch trình                   | `gantt`           | `11-project-task-breakdown.md`         |

### 7.3. Quy ước trình bày

- **Hướng đọc** mặc định: trái → phải (`LR`) cho flowchart, trên → dưới (`TB`)
  cho hierarchy.
- **Đặt tên node** bằng định danh ngắn không dấu (ASCII), nhãn hiển thị trong
  dấu `[]` hoặc `()` có thể dùng tiếng Việt.
- **Mỗi sơ đồ có 1 chú thích** ngay phía dưới giải thích mục đích & các giả
  định khi đọc.
- **Sơ đồ không thay thế văn bản.** Mỗi sơ đồ kèm 1 đoạn diễn giải.

---

## 8. Bảng & danh sách

- **Bảng** dùng khi: ≥ 3 cột HOẶC danh sách ≥ 4 hàng với cấu trúc lặp lại.
- **Bullet list** dùng cho liệt kê ngắn (≤ 7 mục).
- **Numbered list** dùng khi thứ tự có ý nghĩa (các bước).
- Mọi bảng **PHẢI có hàng tiêu đề** và **dấu căn lề** (`:---`, `:---:`,
  `---:`).

---

## 9. Liên kết & tham chiếu chéo

### 9.1. Quy tắc

- **Mọi tham chiếu chéo PHẢI là link Markdown** trỏ tới anchor cụ thể, không
  phải "xem tài liệu 03".
- Link tương đối, dùng dấu `/`, không dùng đường dẫn tuyệt đối hay URL của
  trình duyệt.
- Khi tham chiếu một ID (FR/UC/...), link tới heading chứa ID đó.

### 9.2. Anchor

- Anchor sinh tự động từ heading; quy ước viết heading kèm ID giúp anchor
  ổn định:

  ```markdown
  ### FR-007 — Gợi ý môn học theo chương trình
  ```

  → anchor: `#fr-007--gợi-ý-môn-học-theo-chương-trình` (GitHub đã thay
  khoảng trắng & dấu).

  **Để tránh anchor đổi khi sửa câu chữ**, thêm anchor cố định ngay sau
  heading:

  ```markdown
  ### FR-007 — Gợi ý môn học theo chương trình

  <a id="fr-007"></a>
  ```

  Sau đó link: `[FR-007](#fr-007)`.

---

## 10. Truy ngược (traceability)

Toàn dự án **PHẢI** giữ một ma trận truy ngược tối thiểu:

```
Nhu cầu nghiệp vụ (CLAUDE.md §1)
  └─► Use Case (UC-XXX)
        └─► Yêu cầu chức năng (FR-XXX) / phi chức năng (NFR-XXX)
              ├─► Quyết định kiến trúc (ADR-XXX)
              ├─► Component / API / Bảng CSDL
              └─► Ca kiểm thử (TC-XXX)
```

- Ma trận được duy trì ở phần cuối của `01-srs.md`.
- Khi thêm/xoá một mục bất kỳ trong chuỗi trên, **PHẢI** cập nhật ma trận
  trong **cùng commit**.
- Tài liệu `08-test-plan-acceptance-criteria.md` chịu trách nhiệm phụ: mỗi
  `TC` chỉ rõ phủ FR/UC nào.

---

## 11. Quản lý phiên bản (versioning)

### 11.1. Phiên bản tài liệu

- Dùng **Semantic Versioning** ở mức tài liệu:
  - `MAJOR` — phá vỡ cấu trúc tài liệu hoặc đổi quyết định cốt lõi.
  - `MINOR` — thêm/bớt yêu cầu, mục mới.
  - `PATCH` — sửa chính tả, làm rõ câu chữ, không đổi nghĩa.
- Mỗi lần sửa **PHẢI** cập nhật `Phiên bản` và `Ngày cập nhật` trong
  [bảng §1.1 *Metadata*](#11-metadata), và thêm dòng mới vào
  [bảng §1.2 *Lịch sử thay đổi*](#12-lịch-sử-thay-đổi-changelog).

### 11.2. Trạng thái

| Trạng thái   | Ý nghĩa                                                   |
| ------------ | --------------------------------------------------------- |
| `Draft`      | Đang viết, có thể thay đổi lớn.                           |
| `Review`     | Đã sẵn sàng để duyệt; không tự ý sửa nữa.                 |
| `Approved`   | Đã được người duyệt phê duyệt.                            |
| `Superseded` | Đã có bản thay thế — ghi rõ tài liệu thay thế ở đầu file. |

### 11.3. Quy trình review

1. Tác giả đặt `Trạng thái = Review` và mở một pull request.
2. **Tối thiểu 1 người trong vai trò *Người duyệt*** phải approve.
3. Khi merge: đổi `Trạng thái` sang `Approved`, ghi tên người duyệt vào bảng
   metadata, bump phiên bản, cập nhật `Ngày cập nhật`.

---

## 12. Quy ước nội dung

### 12.1. Ngôn ngữ

- **Tiếng Việt** là ngôn ngữ chính. Câu văn ngắn, chủ ngữ rõ.
- Thuật ngữ tiếng Anh giữ nguyên khi không có bản dịch chuẩn — _in nghiêng_
  lần đầu xuất hiện trong mỗi tài liệu, kèm chú thích tiếng Việt trong ngoặc:
  _use case_ (ca sử dụng), _endpoint_ (điểm cuối).
- Định danh kỹ thuật (tên biến, bảng, API, header) **luôn dùng tiếng Anh**.

### 12.2. Giọng văn

- **Chủ động > bị động.** "Hệ thống ghi log" chứ không "log được ghi".
- Tránh sáo ngữ: "phù hợp", "linh hoạt", "tối ưu", "mạnh mẽ" — thay bằng
  con số.
- Mỗi câu một ý. Câu phức nên gãy thành 2.

### 12.3. Đo lường

- **Số có đơn vị.** "Thời gian phản hồi ≤ 300 ms ở p95" chứ không "phản hồi
  nhanh".
- **Phân vị thay vì trung bình** cho hiệu năng (p50, p95, p99).
- Khi nêu ngưỡng định lượng cho AI, kèm: bộ dữ liệu đánh giá, cách đo,
  ngưỡng đậu.

---

## 13. Quy tắc xây dựng phần mềm (áp dụng khi viết tài liệu thiết kế)

Phần này KHÔNG phải coding standard cho mã nguồn. Nó là **các nguyên tắc thiết
kế** mà các tài liệu HLD/LLD/API/DB **PHẢI** bám theo. Coding standard sẽ
được viết riêng khi mã nguồn xuất hiện (gắn với `09-deployment-operation-standard.md`).

### 13.1. Nguyên tắc

1. **Một nguồn sự thật.** Dữ liệu học vụ (sinh viên, học phần, lớp, học kỳ)
   có duy nhất một nguồn ghi. Các thành phần khác đọc qua API/CSDL chung,
   không tự sao chép.
2. **Tách layer rõ ràng.** Trình bày (UI) ↔ ứng dụng (use case) ↔ miền
   (domain) ↔ hạ tầng (DB, dịch vụ ngoài). Phụ thuộc một chiều từ ngoài vào
   miền.
3. **API là hợp đồng.** Mọi tương tác giữa container đi qua API có khai báo
   trong `05-api-specification.md`. Không "đi cửa sau" qua bảng dữ liệu.
4. **Idempotency cho mutation.** Mọi endpoint thay đổi trạng thái quan trọng
   (đăng ký học, thanh toán sandbox) **PHẢI** chấp nhận `Idempotency-Key`.
5. **Người duyệt cho AI ra ngoài.** Mọi output AI gửi đến người dùng cuối
   (email, thông báo) **PHẢI** qua bước duyệt — phản ánh trong sequence
   diagram của `03-lld.md`.
6. **Trích dẫn nguồn cho AI hội thoại.** Output hỏi đáp học vụ **PHẢI** kèm
   tham chiếu tới nguồn (quy chế, biểu mẫu, học phần).
7. **Quan trắc từ đầu.** Mỗi use case quan trọng có log có cấu trúc
   (structured log) và ít nhất 1 metric — khai báo trong
   `09-deployment-operation-standard.md`.
8. **An toàn theo mặc định.** Mặc định **từ chối** quyền truy cập; cấp quyền
   tường minh trong `07-security-permission-design.md`.
9. **Sandbox tách biệt.** Mọi đoạn mã đụng tới thanh toán **PHẢI** chạy trong
   môi trường sandbox; **CẤM** xuất hiện secret/token thật trong tài liệu
   hoặc mã.
10. **Không tối ưu sớm.** Chỉ tối ưu khi có NFR cụ thể yêu cầu, hoặc có chỉ
    số đo lường cho thấy cần.

### 13.2. Khi vi phạm

Nếu một tình huống yêu cầu vi phạm một trong 10 nguyên tắc trên (ví dụ: hệ
thống bên ngoài không hỗ trợ idempotency), **PHẢI** viết ADR ghi rõ:

- Bối cảnh buộc phải vi phạm.
- Hệ quả tiêu cực chấp nhận được.
- Cách giảm thiểu rủi ro.
- Điều kiện để quay lại tuân thủ.

---

## 14. Quy ước thuật ngữ (glossary)

Mỗi tài liệu lớn (`01-srs.md`, `04-database-design.md`) **PHẢI** có mục
_Glossary_ riêng. Khi một thuật ngữ xuất hiện ở nhiều tài liệu với nghĩa khác
nhau, **PHẢI** thống nhất trong `01-srs.md §glossary` rồi liên kết về đó.

Định nghĩa thuật ngữ trong glossary theo cấu trúc:

```markdown
**Lớp học phần** (_Course offering_) — một lần mở học phần X trong học kỳ Y
với giảng viên, phòng học, thời gian xác định. Sinh viên đăng ký vào _lớp
học phần_, không phải _học phần_.
```

---

## 15. Tham chiếu

- [`../../CLAUDE.md`](../../CLAUDE.md) — phạm vi & ranh giới dự án.
- [`../agents.md`](../agents.md) — chỉ dẫn cho agent viết tài liệu.
- ISO/IEC/IEEE 29148:2018 — Systems and software engineering — Life cycle
  processes — Requirements engineering.
- ISO/IEC/IEEE 42010:2022 — Software, systems and enterprise — Architecture
  description.
- C4 Model — <https://c4model.com/>.
- OpenAPI Specification 3.1 — <https://spec.openapis.org/oas/v3.1.0>.
- Michael Nygard, "Documenting Architecture Decisions" (2011).
- RFC 2119 / RFC 8174 — Key words for use in RFCs to Indicate Requirement
  Levels.
- OWASP Application Security Verification Standard (ASVS).
