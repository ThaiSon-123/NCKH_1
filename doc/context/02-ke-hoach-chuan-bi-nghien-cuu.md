# 02 — Kế hoạch chuẩn bị & các phần cần làm cho nghiên cứu AI

## 1. Thông tin tài liệu

### 1.1. Metadata

| Thuộc tính         | Giá trị                                                                                                |
| ------------------ | ------------------------------------------------------------------------------------------------------ |
| Tên tài liệu       | Kế hoạch chuẩn bị & các phần cần làm cho nghiên cứu AI                                                 |
| Mã tài liệu        | 02-ke-hoach-chuan-bi-nghien-cuu                                                                        |
| Dự án              | Nền tảng tích hợp hỗ trợ tổ chức đào tạo (NCKH_1)                                                      |
| Phiên bản          | v0.1.0                                                                                                 |
| Trạng thái         | Draft                                                                                                  |
| Người viết         | team-architecture                                                                                      |
| Người duyệt        | (chưa duyệt)                                                                                           |
| Ngày tạo           | 2026-05-27                                                                                             |
| Ngày cập nhật      | 2026-05-27                                                                                             |
| Tài liệu liên quan | [`../../CLAUDE.md`](../../CLAUDE.md), [`../SDLC/00-quy-chuan.md`](../SDLC/00-quy-chuan.md), [`01-muc-tieu-nghien-cuu-ai.md`](01-muc-tieu-nghien-cuu-ai.md) |

### 1.2. Lịch sử thay đổi (Changelog)

| Phiên bản | Ngày       | Tác giả           | Thay đổi                                                                                       |
| --------- | ---------- | ----------------- | ---------------------------------------------------------------------------------------------- |
| 0.1.0     | 2026-05-27 | team-architecture | Bản nháp đầu tiên — chốt 5 phần chuẩn bị (dataset, verifier, API, eval, human study), 6 phase lộ trình. |

---

Tài liệu này chốt **các phần cần chuẩn bị** và **lộ trình thực thi** cho
nghiên cứu AI đã định hướng trong [`01-muc-tieu-nghien-cuu-ai.md`](01-muc-tieu-nghien-cuu-ai.md).

Đây không phải `11-project-task-breakdown.md` (file đó dành cho toàn bộ
phần mềm nghiệp vụ). Tài liệu này tập trung vào **nhánh nghiên cứu AI** —
khi viết `11-task` về sau, các mục ở đây sẽ trở thành một _epic_ con.

---

## 2. Phạm vi tài liệu

### 2.1. Phần "Cần chuẩn bị" trả lời câu hỏi

- Có những **tài nguyên / hạ tầng** nào phải sẵn sàng **trước** khi bắt đầu
  thí nghiệm?
- Mỗi tài nguyên gồm những thành phần gì, ai chịu trách nhiệm, đầu ra
  trông như thế nào?
- Tiêu chí "đã sẵn sàng" của từng tài nguyên là gì?

### 2.2. Phần "Cần làm" trả lời câu hỏi

- Lộ trình từ ngày bắt đầu tới ngày báo cáo kết quả gồm những giai đoạn nào?
- Mỗi giai đoạn có cột mốc (milestone) gì để biết đã qua?
- Giai đoạn nào phụ thuộc giai đoạn nào?

### 2.3. Tài liệu này KHÔNG quyết

- Lịch tuyệt đối theo ngày (chốt khi viết `11-project-task-breakdown.md`).
- Phân công nhân lực cụ thể (chốt với người hướng dẫn / nhóm).
- Chi tiết kỹ thuật của từng thành phần (thuộc các tài liệu SDLC).

---

## 3. Các phần cần chuẩn bị

### 3.1. Dataset VACS

**VACS** = _Vietnamese Academic Constraint Set_ — bộ gold set đánh giá γ/α/β.

#### 3.1.1. Đầu ra mong muốn

| Hạng mục              | Yêu cầu tối thiểu (MVP)                          | Mục tiêu mở rộng        |
| --------------------- | ------------------------------------------------ | ----------------------- |
| Số examples           | 300                                              | 500                     |
| Phân tầng độ khó      | Easy: 100, Medium: 100, Hard: 100                | thêm 50 ở mỗi tầng      |
| Đa dạng người yêu cầu | 3 persona (SV năm 1-2, SV năm 3-4, SV ngoại đạo) | thêm SV có hoàn cảnh đặc biệt (đi làm, học bù) |
| Định dạng             | JSONL, 1 dòng / example, có schema cố định       | + bản CSV để mở public  |
| Inter-rater κ         | ≥ 0.7 trên mẫu kiểm tra 30 examples              | ≥ 0.8                   |

#### 3.1.2. Schema 1 example

```jsonc
{
  "id": "VACS-001",
  "difficulty": "easy",
  "persona": "SV-year-3",
  "input_nl": "Em muốn học 18 tín chỉ kỳ này, tránh thứ 7, không học sau 5h chiều...",
  "context": {
    "student_id": "...",
    "completed_courses": ["CS101", "MA102"],
    "current_program": "CNTT-K22"
  },
  "constraints_structured": {
    "hard": [...],
    "soft": [...]
  },
  "gold_solutions": [
    { "schedule": [...], "rationale": "..." }
  ],
  "soft_rubric": {
    "spread_evenly": 0.8,
    "morning_preference": 0.7
  }
}
```

> Schema chính thức được cố định trước khi annotate **example thứ 31**;
> nếu cần đổi sau đó, **PHẢI** bump version dataset và tái annotate batch
> đã làm.

#### 3.1.3. Quy trình annotate

1. **Soạn 30 examples mẫu** bởi tác giả chính → mang đi review với cố vấn / sinh viên.
2. **Thống nhất rubric** cho soft criteria (1 trang).
3. **Tuyển 2 annotator** (sinh viên năm 3-4 cùng ngành).
4. **Annotate song song 30 examples** → đo Cohen's κ → nếu < 0.7 quay lại 2.
5. **Mở rộng lên 300** chia đều giữa 2 annotator + 10% chéo để theo dõi κ liên tục.
6. **Author review pass** toàn bộ trước khi lock dataset cho thí nghiệm.

#### 3.1.4. Tiêu chí "đã sẵn sàng"

- [ ] Schema đã lock và tài liệu hoá.
- [ ] ≥ 300 examples, đủ 3 tầng độ khó.
- [ ] Cohen's κ ≥ 0.7 trên batch kiểm tra cuối.
- [ ] Author đã review pass 100%.
- [ ] Dataset được commit vào kho mã (hoặc lưu trữ riêng có liên kết).

---

### 3.2. Verifier

Hàm thuần kiểm tra ràng buộc trên lời giải — **không phải solver**. Xem
[`01-muc-tieu-nghien-cuu-ai.md §4.2`](01-muc-tieu-nghien-cuu-ai.md).

#### 3.2.1. Đầu ra mong muốn

- Module Python độc lập, không phụ thuộc API LLM.
- Test coverage ≥ 90% cho các nhánh kiểm tra ràng buộc.
- Thời gian check 1 lịch ≤ 100ms (để có thể loop trong γ₃ mà không nghẽn).

#### 3.2.2. Phạm vi ràng buộc verifier check được

| Loại                          | Ví dụ                                              | Verifier bắt buộc check |
| ----------------------------- | -------------------------------------------------- | ----------------------- |
| Trùng lịch (slot collision)   | 2 môn cùng tiết, cùng thứ                          | **PHẢI**                |
| Tiên quyết (prerequisite)     | Phải hoàn thành CS101 trước khi đăng ký CS201      | **PHẢI**                |
| Khối lượng (credit cap)       | ≤ 24 TC / kỳ                                       | **PHẢI**                |
| Lớp đã đầy                    | Số chỗ trống ≥ 1                                   | **PHẢI**                |
| Trùng giảng viên              | GV X không thể dạy 2 lớp cùng tiết                 | **PHẢI**                |
| Trùng phòng                   | Phòng P101 chỉ có 1 lớp cùng tiết                  | **PHẢI**                |
| Sở thích tránh slot           | "Tránh sáng sớm" — tiết 1 thì trừ điểm             | **NÊN** (cho soft score) |
| Cân tải tuần                  | Phân bố TC giữa các ngày                           | **NÊN**                  |

#### 3.2.3. Tiêu chí "đã sẵn sàng"

- [ ] Module verify được commit + có CI chạy unit test.
- [ ] API public: `verify(schedule, constraints) -> {ok, violations, soft_score}`.
- [ ] Có 50 test case (mix valid / invalid) viết tay.
- [ ] Latency p95 ≤ 100ms trên dataset test.

---

### 3.3. API LLM Backbone

#### 3.3.1. Đầu ra mong muốn

- 2 provider khác nhau, đăng ký xong, có API key được quản lý qua biến môi trường.
- Module client wrapper thống nhất interface: `call(provider, model, messages, ...)`.
- Logging mỗi call: timestamp, provider, model, prompt hash, token in/out, latency, cost.
- Budget alert ở 50% / 80% / 100% ngưỡng tháng.

#### 3.3.2. Quyết định cần chốt (đưa vào ADR)

| Mã quyết định | Câu hỏi                                                  | Hạn chốt              |
| ------------- | -------------------------------------------------------- | --------------------- |
| D-API-01      | Provider flagship: Anthropic Claude hay OpenAI GPT?      | Trước Phase 2.        |
| D-API-02      | Provider efficient: Cùng nhà flagship hay khác nhà?      | Cùng D-API-01.        |
| D-API-03      | Pin version (`-2025-XX` snapshot) hay rolling tag?       | Trước Phase 4.        |
| D-API-04      | Caching: dùng prompt caching không (giảm cost RAG)?      | Trước Phase 3.        |
| D-API-05      | Budget tổng cho toàn dự án.                              | Trước Phase 1.        |

#### 3.3.3. Tiêu chí "đã sẵn sàng"

- [ ] 2 provider hoạt động (gọi được "hello world").
- [ ] Wrapper client có test integration đi qua cả 2 provider.
- [ ] Cost dashboard / log file đã có.
- [ ] Budget alert đã được cấu hình.

---

### 3.4. Hạ tầng đánh giá (evaluation infrastructure)

#### 3.4.1. Đầu ra mong muốn

- Script `run_experiment.py` chấp nhận: config cấu hình (γ₀…γ₅, α, β), input dataset, output dir.
- Mỗi run sinh: file kết quả từng example + bảng tổng hợp metric + log đầy đủ.
- Reproducibility: seed cố định cho mọi random op, version pin cho thư viện.
- Bảng tổng hợp so sánh nhiều cấu hình (matrix comparison) sinh tự động.

#### 3.4.2. Thành phần

| Thành phần                | Mô tả                                                         |
| ------------------------- | ------------------------------------------------------------- |
| Config loader             | Đọc cấu hình YAML cho 1 cấu hình thí nghiệm.                  |
| Pipeline runner           | Chạy 1 example qua pipeline được chọn (α/β/γ).                |
| Metric collector          | Tính M1-M6 (xem [`01-muc-tieu-nghien-cuu-ai.md §5.3`](01-muc-tieu-nghien-cuu-ai.md#53-chỉ-số-đo)). |
| Result store              | JSONL/SQLite cho per-example results + tổng hợp.              |
| Stat analyser             | Paired t-test, Wilcoxon, CI, Cohen's d.                       |
| Report renderer           | Sinh bảng Markdown + biểu đồ (matplotlib) từ result store.    |

#### 3.4.3. Tiêu chí "đã sẵn sàng"

- [ ] Pipeline runner chạy được trên dataset 5 example và sinh ra metric tự động.
- [ ] Stat analyser được test với data giả lập có hiệu ứng đã biết.
- [ ] Report sinh được từ ≥ 2 cấu hình so sánh.

---

### 3.5. Human study

#### 3.5.1. Đầu ra mong muốn

- Protocol chính thức: tóm tắt mục tiêu, quy trình, thông báo cho người tham gia, đồng ý tham gia.
- Form khảo sát (Google Form / app riêng) với 3 lịch hiển thị ẩn nguồn α/β/γ.
- Cỡ mẫu **n ≥ 30** sinh viên năm 2-4 cùng ngành/trường.

#### 3.5.2. Quy trình triển khai

1. **Tuần W-2**: Hoàn thiện protocol + form thử nội bộ (n=3).
2. **Tuần W-1**: Pilot 5 SV → tinh chỉnh form (câu hỏi gây nhầm? thời lượng?).
3. **Tuần W0**: Tuyển n = 30-40 (đề phòng dropout).
4. **Trong 2 tuần**: Mỗi SV làm 3 lịch ẩn nguồn, ghi UPS + comment.
5. **Tuần W3**: Phân tích, đối chiếu với metric tự động.

#### 3.5.3. Tiêu chí "đã sẵn sàng"

- [ ] Protocol được người hướng dẫn duyệt.
- [ ] Form pilot xong, fix các điểm nhầm.
- [ ] Danh sách ứng viên ≥ 35 SV với liên hệ.
- [ ] Cơ chế random hoá thứ tự hiển thị α/β/γ đã test.

---

## 4. Lộ trình thực thi (6 phase)

### 4.1. Đồ thị phụ thuộc

```
Phase 0  ──►  Phase 1  ──►  Phase 2  ──►  Phase 3
(Foundation)  (Dataset)     (Verifier+α)   (β: hybrid)
                                                 │
                                                 ▼
                                            Phase 4  ──►  Phase 5  ──►  Phase 6
                                            (γ ablation)   (Benchmark)   (Writing)
```

Mỗi phase có **cột mốc đầu vào** (điều kiện để bắt đầu) và **cột mốc đầu ra**
(điều kiện để phase này hoàn thành).

### 4.2. Bảng phase tổng quan

| Phase | Tên                              | Đầu vào yêu cầu                    | Đầu ra cột mốc                                                       | Ước lượng |
| ----- | -------------------------------- | ---------------------------------- | -------------------------------------------------------------------- | --------- |
| 0     | Foundation                       | (gốc)                              | SRS + HLD + ADR-001 (chọn LLM provider) đã ở `Review`.               | 4 tuần    |
| 1     | Dataset construction             | Phase 0                            | VACS-300 ready + Cohen's κ ≥ 0.7.                                    | 6 tuần    |
| 2     | Verifier + α baseline            | Phase 0 (chỉ HLD)                  | Verifier ổn định + α chạy trên VACS-300 → có baseline SVR.           | 4 tuần    |
| 3     | β hybrid implementation          | Phase 1 + Phase 2                  | β chạy được trên VACS-300, có metric M1-M5.                         | 3 tuần    |
| 4     | γ ablation (γ₀ → γ₄, ± γ₅)        | Phase 2 + Phase 3                  | γ₀-γ₄ chạy xong, có matrix kết quả.                                  | 6 tuần    |
| 5     | Benchmark + human study          | Phase 4 (γ₃ stable)                | M1-M6 đầy đủ cho 3 hệ; human study n ≥ 30 xong; phân tích thống kê. | 4 tuần    |
| 6     | Writing                          | Phase 5                            | Luận văn + paper draft + ADR cập nhật.                               | 4 tuần    |

> Ước lượng theo **tuần làm việc**, không phải tuần lịch. Buffer 20% chưa
> tính trong bảng — bù khi viết `11-project-task-breakdown.md`.

### 4.3. Phase 0 — Foundation

**Mục đích**: Khoá phạm vi, chốt công cụ, đồng thuận tài liệu trước khi đụng code.

| Task                                       | Đầu ra                                              |
| ------------------------------------------ | --------------------------------------------------- |
| Hoàn thiện `01-srs.md` (kèm `AI-XXX`)      | SRS bản `Review`.                                   |
| Hoàn thiện `02-hld.md` (containers + AI strategy) | HLD bản `Review`.                                |
| Viết `ADR-001` — chọn cặp LLM provider     | ADR `Approved`.                                     |
| Setup repo: CI, lint, test infra           | CI xanh trên repo trống.                            |
| Đăng ký API keys cho 2 provider, set budget alert | Có sandbox call thử thành công.                |

**Cột mốc đầu ra Phase 0**:

- [ ] SRS / HLD ở `Review`.
- [ ] ADR-001 `Approved`.
- [ ] Repo có CI xanh.
- [ ] 2 API key hoạt động, có budget alert.

### 4.4. Phase 1 — Dataset construction (chi tiết §3.1)

**Mục đích**: Có VACS-300 sẵn sàng để benchmark.

| Tuần | Hoạt động                                                  |
| ---- | ---------------------------------------------------------- |
| 1    | Soạn 30 examples mẫu + rubric soft criteria.               |
| 2    | Pilot annotation với 2 SV → đo κ → tinh chỉnh schema/rubric. |
| 3-5  | Annotate VACS-300 song song; theo dõi κ mỗi 50 examples.   |
| 6    | Author review pass + lock dataset v1.0.                    |

**Cột mốc đầu ra Phase 1**:

- [ ] VACS-300 đạt schema lock.
- [ ] κ ≥ 0.7 trên batch test.
- [ ] Author review 100%.

### 4.5. Phase 2 — Verifier + α baseline

**Mục đích**: Có ground truth (verifier) + baseline solver (α) trên cùng VACS.

| Tuần | Hoạt động                                                                 |
| ---- | ------------------------------------------------------------------------- |
| 1-2  | Implement verifier (xem §3.2).                                            |
| 3    | Implement α — OR-Tools CP-SAT cho gen schedule, gắn verifier vào pipeline. |
| 4    | Chạy α trên VACS-300, log kết quả, sinh báo cáo baseline.                 |

**Cột mốc đầu ra Phase 2**:

- [ ] Verifier coverage ≥ 90% test, latency ≤ 100ms p95.
- [ ] α có SVR baseline được công bố nội bộ.
- [ ] Eval infrastructure (§3.4) chạy được trên cấu hình α.

### 4.6. Phase 3 — β hybrid (LLM → tool → solver)

**Mục đích**: Có hệ trung gian để so sánh với γ.

| Tuần | Hoạt động                                                              |
| ---- | ---------------------------------------------------------------------- |
| 1    | Implement bộ NL→constraint extractor (LLM với schema constrained output). |
| 2    | Wire vào solver có sẵn (Phase 2).                                      |
| 3    | Chạy β trên VACS-300, thu metric M1-M5.                                |

**Cột mốc đầu ra Phase 3**:

- [ ] β chạy end-to-end trên VACS-300.
- [ ] Có log đầy đủ + bảng metric M1-M5.

### 4.7. Phase 4 — γ ablation (γ₀ → γ₄)

**Mục đích**: Trả lời RQ-AI bằng cách so sánh γ ở nhiều cấu hình.

| Tuần | Hoạt động                                                            |
| ---- | -------------------------------------------------------------------- |
| 1    | γ₀ — LLM thô + prompt cơ bản; tinh chỉnh prompt.                     |
| 2    | γ₁ — thêm CoT; γ₂ — thêm RAG (cần chuẩn bị corpus quy chế/CTĐT).    |
| 3-4  | γ₃ — verifier loop; benchmarking + tinh chỉnh stop criteria.         |
| 5    | γ₄ — Best-of-N + Self-Consistency; tinh chỉnh N.                     |
| 6    | Gom toàn bộ kết quả, sinh bảng so sánh γ₀ → γ₄.                      |

**Cột mốc đầu ra Phase 4**:

- [ ] γ₀-γ₄ đều chạy được trên VACS-300.
- [ ] Bảng so sánh có CI + p-value.
- [ ] Đã quyết: có làm γ₅ hay không (theo budget thời gian còn lại).

### 4.8. Phase 5 — Benchmark + human study

**Mục đích**: Hoàn thiện bộ kết quả 3-way + UPS từ người dùng thật.

| Tuần | Hoạt động                                                           |
| ---- | ------------------------------------------------------------------- |
| 1    | Hoàn thiện protocol + pilot human study (§3.5).                    |
| 2-3  | Triển khai human study với n ≥ 30.                                  |
| 4    | Tổng hợp UPS, phân tích thống kê tổng hợp M1-M6.                    |

**Cột mốc đầu ra Phase 5**:

- [ ] Human study đủ n = 30.
- [ ] M1-M6 tính xong cho α, β, γ (cấu hình tốt nhất).
- [ ] H1, H2, H3 được phán xét có/không đậu.

### 4.9. Phase 6 — Writing

**Mục đích**: Đầu ra cuối cùng — luận văn + paper.

| Tuần | Hoạt động                                                                |
| ---- | ------------------------------------------------------------------------ |
| 1    | Hoàn thiện chương "Phương pháp" + "Thí nghiệm" trong luận văn.            |
| 2    | Hoàn thiện chương "Kết quả" + "Phân tích".                                |
| 3    | Hoàn thiện toàn bộ + làm slides bảo vệ.                                  |
| 4    | Đóng băng + nộp; có thể tách paper hội nghị riêng (tuỳ định hướng).      |

**Cột mốc đầu ra Phase 6**:

- [ ] Luận văn sẵn sàng bảo vệ.
- [ ] Tất cả ADR ở trạng thái `Approved`.
- [ ] Toàn bộ tài liệu SDLC sync với code (không có drift).

---

## 5. Stretch goal γ₅ — Fine-tune

**Chỉ kích hoạt** khi Phase 4 kết thúc trước hạn ≥ 2 tuần.

### 5.1. Phạm vi

- Base model: 1 trong { Qwen 2.5 7B, Vistral 7B, Llama 3 8B } — quyết khi
  kích hoạt, ghi vào ADR mới.
- Phương pháp: **LoRA** (rank 16-32) hoặc **QLoRA 4-bit** nếu compute hạn chế.
- Train set: VACS-300 (chia 240/30/30 train/val/test) hoặc mở rộng VACS-500.

### 5.2. Tiêu chí kích hoạt

| Điều kiện                                | Bắt buộc |
| ---------------------------------------- | -------- |
| Phase 4 hoàn tất                         | có       |
| Còn ≥ 8 tuần trước hạn nộp luận văn      | có       |
| Có GPU (cloud A100 hoặc local 4090)      | có       |
| Có dataset đã lock (không sửa khi train) | có       |

### 5.3. Tiêu chí dừng / bỏ

- Nếu **2 tuần** đầu của γ₅ chưa có baseline so sánh được với γ₃, **dừng**.
- Nếu γ₅ thấp hơn γ₃ ≥ 5% SVR, **chấp nhận kết quả tiêu cực** và ghi vào báo cáo.

---

## 6. Phụ thuộc tài nguyên

| Tài nguyên                                  | Cần khi                | Nguồn dự kiến                       |
| ------------------------------------------- | ---------------------- | ----------------------------------- |
| Quy chế đào tạo + chương trình đào tạo      | Phase 1 (làm corpus + tham chiếu) | Phòng đào tạo khoa/trường |
| Danh sách lớp học phần mẫu                  | Phase 2 (input α/verifier) | Phòng đào tạo                  |
| Sinh viên cho annotation (n=2)              | Phase 1                | Bộ môn / CLB sinh viên              |
| Sinh viên cho human study (n=30)            | Phase 5                | Lớp / CVHT giới thiệu               |
| API budget (ước tính)                       | Phase 3-5              | Tự túc / quỹ NCKH (cần xin trước Phase 0) |
| GPU (chỉ nếu γ₅ kích hoạt)                  | Stretch                | Cloud A100 hoặc local 4090          |
| Người duyệt tài liệu                        | Mỗi phase              | Người hướng dẫn / nhóm              |

---

## 7. Rủi ro vận hành (bổ sung cho `01-muc-tieu-nghien-cuu-ai.md §9`)

| Mã        | Rủi ro vận hành                                                                  | Cách xử lý                                                          |
| --------- | -------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `RISK-101` | Annotator bỏ giữa chừng → Phase 1 trễ.                                          | Tuyển 3 thay vì 2; chuẩn bị backup annotator.                       |
| `RISK-102` | Khoa không cấp dữ liệu CTĐT thật → corpus RAG rỗng.                              | Xin sớm ở Phase 0; chuẩn bị fallback dùng dữ liệu giả lập có cấu trúc tương đương. |
| `RISK-103` | API bị block / quota giới hạn lúc benchmark.                                      | Provider thứ 2 là backup; giảm batch size.                          |
| `RISK-104` | Verifier sót case → SVR đo sai.                                                  | Test verifier với corner case có ground truth trước khi dùng cho benchmark. |
| `RISK-105` | Một phase trễ kéo cả phase sau.                                                  | Mỗi cột mốc đầu phase có "go/no-go review"; cắt scope thay vì trễ.  |

---

## 8. Bảng tham chiếu chéo các thành phần

| Thành phần            | Định nghĩa                  | Chuẩn bị ở §           | Dùng ở Phase         |
| --------------------- | --------------------------- | ---------------------- | -------------------- |
| VACS-300              | [§3.1](#31-dataset-vacs)    | §3.1                   | 3, 4, 5              |
| Verifier              | [§3.2](#32-verifier)        | §3.2                   | 2, 3, 4              |
| API wrapper           | [§3.3](#33-api-llm-backbone) | §3.3                  | 3, 4, 5              |
| Eval infrastructure   | [§3.4](#34-hạ-tầng-đánh-giá-evaluation-infrastructure) | §3.4 | 2, 3, 4, 5 |
| Human study setup     | [§3.5](#35-human-study)     | §3.5                   | 5                    |

---

## 9. Tham chiếu

- [`../../CLAUDE.md`](../../CLAUDE.md) — phạm vi & ranh giới dự án.
- [`../SDLC/00-quy-chuan.md`](../SDLC/00-quy-chuan.md) — quy chuẩn tài liệu.
- [`../agents.md`](../agents.md) — chỉ dẫn cho agent.
- [`01-muc-tieu-nghien-cuu-ai.md`](01-muc-tieu-nghien-cuu-ai.md) — quyết định về hướng nghiên cứu AI, các metric M1-M6, giả thuyết H1-H3.
- PMBOK 7 — Work Breakdown Structure (áp dụng khi viết `11-project-task-breakdown.md`).
- Bảng phase ở §4 sẽ được _expand_ thành WBS chi tiết trong `11-project-task-breakdown.md`.
