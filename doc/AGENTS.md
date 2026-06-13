# agents.md — Chỉ dẫn cho agent khi viết tài liệu

File này hướng dẫn **mọi agent (Claude và các dẫn xuất)** soạn tài liệu trong thư
mục `doc/`. Mục tiêu: bộ tài liệu **đầy đủ, nhất quán, kiểm chứng được**, không
"giấy tờ cho có".

> Tuân thủ bắt buộc: [`doc/SDLC/00-quy-chuan.md`](SDLC/00-quy-chuan.md). File
> hiện tại nói **làm gì & theo thứ tự nào**; quy chuẩn nói **viết theo hình
> thức nào**.

---

## 1. Trước khi viết bất kỳ tài liệu nào

Đọc theo đúng thứ tự:

1. [`../CLAUDE.md`](../CLAUDE.md) — phạm vi, ranh giới, ngôn ngữ, ràng buộc cứng.
2. [`SDLC/00-quy-chuan.md`](SDLC/00-quy-chuan.md) — chuẩn trình bày, đặt ID,
   từ khoá nghĩa vụ (PHẢI/NÊN/CÓ THỂ), diagram, traceability.
3. Toàn bộ các file đã hoàn thành (không phải bản nháp) trong `SDLC/` —
   để giữ tính nhất quán liên tài liệu.
4. Các tài liệu trong `doc/context/`:
   - [`context/GLOSSARY.md`](context/GLOSSARY.md) — **thuật ngữ dùng chung** (nguồn
     sự thật duy nhất; liên kết về đây, **không nhân bản** trong tài liệu khác).
   - [`context/DOMAIN-MAP.md`](context/DOMAIN-MAP.md) — **bản đồ miền & ma trận truy
     ngược** (UC ↔ FR ↔ NFR ↔ AI).
   - [`context/PROJECT-STATE.md`](context/PROJECT-STATE.md) — **trạng thái dự án**,
     giai đoạn hiện tại, quyết định đang mở, gap cần bổ sung.
   - [`context/01-muc-tieu-nghien-cuu-ai.md`](context/01-muc-tieu-nghien-cuu-ai.md),
     [`context/02-ke-hoach-chuan-bi-nghien-cuu.md`](context/02-ke-hoach-chuan-bi-nghien-cuu.md)
     — bối cảnh & lộ trình nghiên cứu AI.

**Không bắt đầu viết** nếu một trong bốn mục trên chưa được đọc. Nếu một tài
liệu phụ thuộc còn rỗng, **dừng lại và hỏi người dùng** thay vì tự suy đoán.

---

## 2. Thứ tự viết tài liệu (đồ thị phụ thuộc)

Các file SDLC **không độc lập** — file sau dùng đầu ra của file trước. Hãy viết
theo lượt sau, và **không bỏ qua bước**:

```
00-quy-chuan ──► 01-srs ──┬─► 02-hld ──► 03-lld ─┬─► 04-database
                          │                       ├─► 05-api
                          │                       ├─► 06-ui-ux
                          │                       └─► 07-security
                          ├─► 10-adr (song song, theo từng quyết định)
                          ├─► 08-test  (sau khi 01 & 05 ổn định)
                          ├─► 11-task  (sau khi 02 ổn định)
                          └─► 09-deployment (sau khi 02 & 07 ổn định)
```

Diễn giải:

| #   | File                                  | Phụ thuộc (đọc trước)                                                      |
| --- | ------------------------------------- | -------------------------------------------------------------------------- |
| 00  | `00-quy-chuan.md`                     | (gốc)                                                                      |
| 01  | `01-srs.md`                           | `CLAUDE.md` §1, `context/`                                                 |
| 02  | `02-hld.md`                           | `01-srs.md`                                                                |
| 03  | `03-lld.md`                           | `02-hld.md`                                                                |
| 04  | `04-database-design.md`               | `01-srs.md`, `03-lld.md`                                                   |
| 05  | `05-api-specification.md`             | `03-lld.md`, `04-database-design.md`                                       |
| 06  | `06-ui-ux-flow-specification.md`      | `01-srs.md`, `05-api-specification.md`                                     |
| 07  | `07-security-permission-design.md`    | `01-srs.md` (Use Case), `04-database-design.md`, `05-api-specification.md` |
| 08  | `08-test-plan-acceptance-criteria.md` | `01-srs.md`, `05-api-specification.md`, `06-ui-ux-flow-specification.md`   |
| 09  | `09-deployment-operation-standard.md` | `02-hld.md`, `07-security-permission-design.md`                            |
| 10  | `10-architecture-decision-record.md`  | bắt đầu từ ADR-001 song song với 01–03                                     |
| 11  | `11-project-task-breakdown.md`        | `02-hld.md`, `08-test-plan-acceptance-criteria.md`                         |

---

## 3. Nội dung tối thiểu của từng file

Đây là _nội dung tối thiểu_ mỗi file phải có. Bố cục chung (bảng metadata, lịch
sử thay đổi, tham chiếu) xem [`00-quy-chuan.md §4`](SDLC/00-quy-chuan.md#4-cấu-trúc-chung-của-một-tài-liệu-sdlc).

### 3.1. `01-srs.md` — Software Requirements Specification

- **Bối cảnh & các bên liên quan** (6 nhóm người dùng nghiệp vụ + vai trò kỹ thuật SysAdmin).
- **Đặc tả use case** — mỗi use case có ID `UC-XXX`, actor chính, tiền điều
  kiện, luồng chính, luồng phụ, hậu điều kiện.
- **Yêu cầu chức năng** `FR-XXX` — phát biểu theo "Hệ thống **PHẢI** ..." (xem
  RFC 2119 trong [`00-quy-chuan.md §2`](SDLC/00-quy-chuan.md#2-chuẩn-nền)). Mỗi FR ánh xạ tới ≥ 1 UC.
- **Yêu cầu phi chức năng** `NFR-XXX` — hiệu năng, khả dụng, bảo mật, đa người
  dùng đồng thời, độ chính xác AI (chỉ số định lượng).
- **Yêu cầu cho năng lực AI** `AI-XXX` — **mỗi năng lực AI bắt buộc có 1 chỉ số
  đánh giá định lượng** kèm phương pháp đo & ngưỡng tối thiểu.
- **Ràng buộc & giả định** — bao gồm: sandbox thanh toán, người duyệt AI output,
  miễn/giảm học phí ngoài phạm vi.
- **Glossary** — **không lặp trong SRS**; thuật ngữ dùng chung được duy trì tại
  [`context/GLOSSARY.md`](context/GLOSSARY.md). SRS liên kết về đó (xem `ADR-002`).
- **Traceability matrix** — duy trì tại [`context/DOMAIN-MAP.md`](context/DOMAIN-MAP.md)
  (UC ↔ FR ↔ NFR ↔ AI), **không** đặt trong SRS (xem `ADR-002`).

> Mỗi yêu cầu phải **kiểm chứng được**. Tránh từ "nhanh", "thân thiện", "dễ
> dùng" mà không có số. Ví dụ tốt: "thời gian phản hồi gợi ý môn ≤ 3s ở phân vị
> 95 với 100 người dùng đồng thời".

### 3.2. `02-hld.md` — High-Level Design

- **Sơ đồ ngữ cảnh (C4 Level 1)** — hệ thống & các bên ngoài (email/SMS, cổng
  sandbox, dịch vụ AI).
- **Sơ đồ container (C4 Level 2)** — frontend, backend, CSDL, dịch vụ AI,
  hàng đợi, ...
- **Chiến lược dữ liệu** — nguồn sự thật duy nhất cho dữ liệu học vụ.
- **Chiến lược AI** — RAG, prompt template, nguồn tri thức, kiểm soát chất
  lượng, người duyệt.
- **Chiến lược bảo mật** — phân quyền theo vai trò (6 nhóm nghiệp vụ + SysAdmin).
- **Các quyết định kiến trúc lớn** — link sang `10-architecture-decision-record.md`.
- **Mapping FR/NFR ↔ container**.

### 3.3. `03-lld.md` — Low-Level Design

- **Sơ đồ component (C4 Level 3)** cho từng container quan trọng.
- **Mô hình miền (domain model)** — class/entity, quan hệ.
- **Sequence diagram** cho các luồng cốt lõi (đăng ký học phần, sinh thời khoá
  biểu AI, ước tính học phí, hỏi đáp học vụ có trích dẫn).
- **State machine** cho thực thể có vòng đời rõ (lớp học phần, hoá đơn học phí
  sandbox).
- **Mapping component ↔ FR**.

### 3.4. `04-database-design.md`

- **ER diagram** (Mermaid `erDiagram`).
- **Bảng & thuộc tính** — kiểu dữ liệu, NULL/NOT NULL, default, khoá chính,
  khoá ngoại, unique, check constraint.
- **Chỉ mục (index)** — kèm lý do (truy vấn nào dùng).
- **Chiến lược migration & seed**.
- **Dữ liệu nhạy cảm** — đánh dấu PII, chiến lược mã hoá.

### 3.5. `05-api-specification.md`

- **OpenAPI 3.1** là chuẩn. Có thể nhúng YAML hoặc liên kết tới file ngoài,
  miễn là phiên bản trong tài liệu khớp với mã (tham chiếu ADR nếu khác).
- Nhóm endpoint theo **resource** (sinh viên, học phần, lớp học phần, học phí,
  AI, ...).
- Mỗi endpoint: HTTP method, path, mô tả, request schema, response schema,
  mã lỗi, ai được gọi (vai trò).
- **Rate limit & idempotency** cho endpoint thay đổi trạng thái (đăng ký học,
  thanh toán sandbox).

### 3.6. `06-ui-ux-flow-specification.md`

- **Bản đồ màn hình** cho 6 nhóm người dùng nghiệp vụ + SysAdmin.
- **User flow** — sơ đồ luồng (Mermaid `flowchart`) cho các use case chính.
- **Wireframe / low-fi mockup** — nhúng ảnh hoặc liên kết Figma; nếu chưa có,
  mô tả bằng text + bố cục dạng cây.
- **Trạng thái UI** — empty, loading, error, success.
- **Tính khả dụng (accessibility)** — mức tối thiểu (ví dụ WCAG 2.1 AA cho
  các luồng chính).

### 3.7. `07-security-permission-design.md`

- **Mô hình phân quyền** — RBAC tối thiểu cho các vai trò (xem [`SDLC/01-srs.md §7`](SDLC/01-srs.md#7-actor-và-vai-trò) — 8 vai trò); cân nhắc ABAC cho các
  điều kiện theo ngữ cảnh (giảng viên chỉ thấy lớp mình dạy, v.v.).
- **Ma trận quyền** — vai trò × hành động × tài nguyên.
- **Xác thực** — phương thức (email + mật khẩu, SSO trường, ...).
- **Bảo vệ dữ liệu** — PII, mã hoá at-rest / in-transit.
- **Mô hình mối đe doạ (threat model)** — STRIDE tóm tắt cho 3–5 luồng nhạy
  cảm nhất (đăng ký học, thanh toán sandbox, sinh email AI).
- **Nhật ký kiểm toán (audit log)** — sự kiện nào được ghi, ai xem được.

### 3.8. `08-test-plan-acceptance-criteria.md`

- **Chiến lược test** — đơn vị, tích hợp, e2e, kiểm thử AI.
- **Bộ ca kiểm thử (test cases)** — mỗi case có ID `TC-XXX`, ánh xạ tới FR/UC.
- **Acceptance criteria** cho mỗi epic/sprint — viết theo Given-When-Then.
- **Đánh giá AI** — bộ dữ liệu kiểm thử (gold set), tiêu chí đậu/rớt cho mỗi
  năng lực AI khớp với chỉ số trong `01-srs.md §AI`.
- **Định nghĩa "hoàn thành" (Definition of Done)**.

### 3.9. `09-deployment-operation-standard.md`

- **Môi trường** — local, dev, staging, prod (nếu có).
- **Quy trình triển khai** — CI/CD pipeline, tiêu chí promote.
- **Quan trắc (observability)** — log, metric, trace; cảnh báo gì, ai nhận.
- **Sao lưu & khôi phục**.
- **Vận hành thường ngày** — runbook tóm tắt cho 3–5 sự cố hay gặp.

### 3.10. `10-architecture-decision-record.md`

- Mỗi quyết định là **một section riêng** theo format Michael Nygard:
  `Status`, `Context`, `Decision`, `Consequences`.
- Đánh số `ADR-001`, `ADR-002`, ... — **không tái sử dụng số**.
- Khi một ADR bị thay thế, đặt `Status: Superseded by ADR-XXX` chứ không xoá.

### 3.11. `11-project-task-breakdown.md`

- **WBS** chia theo epic → story → task.
- Mỗi task có: mô tả, công sức ước lượng (story point hoặc giờ), phụ thuộc,
  tiêu chí hoàn thành.
- Ánh xạ task ↔ FR / UC để truy ngược.

---

## 4. Quy tắc viết (rút gọn — chi tiết ở `00-quy-chuan.md`)

1. **Cụ thể, kiểm chứng được.** Không "phù hợp", "linh hoạt", "tối ưu" mà
   không định nghĩa rõ.
2. **Mọi yêu cầu/quyết định có ID** (`FR-XXX`, `UC-XXX`, `ADR-XXX`, ...).
3. **Mọi tham chiếu chéo dùng link Markdown** đến đúng anchor — không "xem ở
   tài liệu khác" chung chung.
4. **Diagram dùng Mermaid** khi có thể (xem [`00-quy-chuan.md §6`](SDLC/00-quy-chuan.md#6-sơ-đồ)).
5. **Bảng dùng cho danh sách có cấu trúc** (≥ 3 cột, ≥ 3 hàng); danh sách ngắn
   thì dùng bullet.
6. **Không sao chép văn bản** giữa các tài liệu — _liên kết_, đừng _nhân bản_.
   Nếu một thông tin xuất hiện ở 2 nơi, sau vài lần sửa hai bản sẽ lệch nhau.
7. **Tiếng Việt là ngôn ngữ chính**; thuật ngữ tiếng Anh giữ nguyên (in
   _nghiêng_ lần đầu, kèm chú thích).

---

## 5. Checklist hoàn thành một tài liệu

Trước khi đánh dấu một tài liệu là "đã xong", kiểm tra:

- [ ] Có **bảng §1.1 _Metadata_** và **bảng §1.2 _Lịch sử thay đổi_** ở đầu
      file đúng [`00-quy-chuan.md §4.1`-`§4.2`](SDLC/00-quy-chuan.md#4-cấu-trúc-chung-của-một-tài-liệu-sdlc).
- [ ] `Phiên bản` & `Ngày cập nhật` trong bảng metadata khớp với dòng mới nhất
      của bảng lịch sử thay đổi.
- [ ] Phụ thuộc đã được đọc và liên kết ngược.
- [ ] Mọi mục bắt buộc trong §3 ở trên đều có.
- [ ] Mọi ID là duy nhất trong file và đúng quy ước.
- [ ] Mọi yêu cầu/ca kiểm thử **kiểm chứng được** (có thể đo, đếm, hoặc
      quan sát kết quả).
- [ ] Không có TODO/`[?]`/`...` còn sót.
- [ ] Diagram render được (kiểm tra bằng cách preview Markdown).
- [ ] Đã cập nhật `changelog` ở cuối file.
- [ ] Đã cập nhật bảng traceability ở [`context/DOMAIN-MAP.md`](context/DOMAIN-MAP.md) nếu thêm/đổi yêu cầu.

---

## 6. Khi nào dừng và hỏi người dùng

- Khi tài liệu phụ thuộc còn **trống hoặc mâu thuẫn** với tài liệu bạn đang
  viết.
- Khi cần **giả định nghiệp vụ** không có trong bối cảnh đã chốt (ví dụ:
  ngưỡng "lớp quá tải" là bao nhiêu? quy chế đăng ký học cho phép tối đa
  bao nhiêu tín chỉ?).
- Khi thay đổi tài liệu có thể **kéo theo sửa ở ≥ 2 tài liệu khác**
  (ví dụ thêm trường vào CSDL kéo theo sửa API và UI).
- Khi cần **ra quyết định kiến trúc** — viết ADR và xin xác nhận trước khi
  áp dụng rộng.

Một câu hỏi rõ ràng tốt hơn một trang tài liệu phải viết lại.

---

## 7. Anti-pattern cần tránh

| Anti-pattern                                 | Tại sao tránh                                    |
| -------------------------------------------- | ------------------------------------------------ |
| Viết "Hệ thống thân thiện, dễ sử dụng"       | Không đo được. Thay bằng tiêu chí cụ thể.        |
| Sao chép cả khối use case từ 01 sang 06      | Hai bản sẽ lệch sau vài lần sửa. Hãy _liên kết_. |
| Tự thêm tính năng "cho hoàn thiện"           | Phá vỡ phạm vi đã chốt (`CLAUDE.md §1.5`).       |
| Đặt ID `FR-1`, `FR-2` không zero-pad         | Sắp xếp lộn xộn khi vượt 10. Dùng `FR-001`.      |
| Viết AI không kèm chỉ số đánh giá            | Phá vỡ tiêu chí thành công (`CLAUDE.md §1.5`).   |
| Sinh tài liệu "đầy đủ" mà chưa đọc phụ thuộc | Mâu thuẫn liên tài liệu. Hỏi trước.              |
