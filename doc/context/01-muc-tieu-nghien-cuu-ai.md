# 01 — Mục tiêu nghiên cứu AI

## 1. Thông tin tài liệu

### 1.1. Metadata

| Thuộc tính         | Giá trị                                                                                                |
| ------------------ | ------------------------------------------------------------------------------------------------------ |
| Tên tài liệu       | Mục tiêu nghiên cứu AI                                                                                 |
| Mã tài liệu        | 01-muc-tieu-nghien-cuu-ai                                                                              |
| Dự án              | Nền tảng tích hợp hỗ trợ tổ chức đào tạo (NCKH_1)                                                      |
| Phiên bản          | v0.1.0                                                                                                 |
| Trạng thái         | Draft                                                                                                  |
| Người viết         | team-architecture                                                                                      |
| Người duyệt        | (chưa duyệt)                                                                                           |
| Ngày tạo           | 2026-05-27                                                                                             |
| Ngày cập nhật      | 2026-05-27                                                                                             |
| Tài liệu liên quan | [`../../CLAUDE.md`](../../CLAUDE.md), [`../SDLC/00-quy-chuan.md`](../SDLC/00-quy-chuan.md), [`02-ke-hoach-chuan-bi-nghien-cuu.md`](02-ke-hoach-chuan-bi-nghien-cuu.md) |

### 1.2. Lịch sử thay đổi (Changelog)

| Phiên bản | Ngày       | Tác giả           | Thay đổi                                                                                   |
| --------- | ---------- | ----------------- | ------------------------------------------------------------------------------------------ |
| 0.1.0     | 2026-05-27 | team-architecture | Bản nháp đầu tiên — chốt hướng nghiên cứu C (Pure LLM end-to-end), khung benchmark α/β/γ, lựa chọn backbone API. |

---

Tài liệu này ghi lại các **quyết định đã chốt** về mục tiêu nghiên cứu AI của
dự án **NCKH_1**, hình thành sau quá trình thảo luận về tính khả thi của ba
hướng đóng góp khoa học (A — đóng góp mô hình, B — đóng góp dataset, C — đóng
góp hệ thống Pure LLM end-to-end).

Tài liệu **KHÔNG** thay thế `01-srs.md`. Nó là nguồn đầu vào để khi viết
SRS, mục _Yêu cầu cho năng lực AI_ (`AI-XXX`) có cơ sở rõ ràng.

---

## 2. Phạm vi tài liệu

### 2.1. Tài liệu này giải quyết câu hỏi gì

- Hướng nghiên cứu AI nào được chốt cho luận văn?
- Khung thí nghiệm so sánh là gì? Đo cái gì?
- LLM backbone lấy từ đâu (API hay tự huấn luyện)?
- Ranh giới giữa **nghiên cứu AI** và **xây dựng hệ thống nghiệp vụ** ở đâu?

### 2.2. Tài liệu này KHÔNG giải quyết

- Chi tiết kiến trúc kỹ thuật (thuộc `02-hld.md`, `03-lld.md`).
- Yêu cầu chức năng / phi chức năng cụ thể (thuộc `01-srs.md`).
- Kế hoạch nhiệm vụ chi tiết (thuộc [`02-ke-hoach-chuan-bi-nghien-cuu.md`](02-ke-hoach-chuan-bi-nghien-cuu.md) và `11-project-task-breakdown.md`).

---

## 3. Bối cảnh quyết định

### 3.1. Hai khung định vị dự án đã được cân nhắc

| Khung                  | Mô tả                                                                            | Tiêu chí thành công                                                  |
| ---------------------- | -------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| Hệ thống **có AI**     | Sản phẩm nghiệp vụ ưu tiên; AI là tính năng bổ trợ.                              | Phần mềm vận hành đúng cho 6 nhóm người dùng × 8 nhóm bài toán con.  |
| **Nghiên cứu AI**      | Dự án mang đóng góp khoa học mới; sản phẩm nghiệp vụ là môi trường đánh giá.    | Có đóng góp định lượng vượt baseline đã công bố.                     |

Hai khung này **không loại trừ nhau**. Quyết định ở §4 cho phép thực hiện cả
hai: phần mềm nghiệp vụ vẫn được xây trọn vẹn, và **một** năng lực AI cụ thể
được đẩy lên thành đối tượng nghiên cứu khoa học có đo lường.

### 3.2. Ba hướng đóng góp AI đã được cân nhắc

| Mã hướng | Tên                                  | Bản chất                                                                  | Trạng thái sau thảo luận                       |
| -------- | ------------------------------------ | ------------------------------------------------------------------------- | ---------------------------------------------- |
| A        | Đóng góp mô hình                     | Huấn luyện / fine-tune một mô hình AI mới cho domain.                     | Không khả thi với tài nguyên luận văn nếu hiểu là pre-train; **chỉ khả thi nếu hiểu là LoRA fine-tune trên model nguồn mở**. |
| B        | Đóng góp dataset / benchmark         | Xây dựng bộ dữ liệu Vietnamese Academic Constraint và benchmark đi kèm.    | Khả thi và **bắt buộc phải có** dù chọn hướng nào — vì C cũng cần dữ liệu để đánh giá. |
| C        | Đóng góp hệ thống Pure LLM end-to-end | LLM xử lý trực tiếp ràng buộc nghiệp vụ, không gọi constraint solver.    | **Được chốt làm hướng chính** (xem §4).        |

### 3.3. Lý do loại bỏ hướng "hệ thống có AI" thuần tuý

Nếu chỉ làm "hệ thống có AI" theo cách phổ biến — _LLM dịch ngôn ngữ tự nhiên
→ trích thông tin → đẩy vào constraint solver (OR-Tools/MiniZinc)_ — thì:

- Đóng góp khoa học **gần bằng không**: kỹ thuật "LLM as front-end to solver"
  đã có nhiều công bố.
- Cộng đồng AI/OR đã có baseline cho mọi phần.
- Không có cơ chế để chứng minh giá trị riêng so với bài toán đã giải.

→ Hướng này có thể là **một nhánh so sánh** (β trong §5), nhưng **không phải
hướng đóng góp chính**.

---

## 4. Quyết định chốt: Hướng nghiên cứu C — Pure LLM end-to-end

### 4.1. Phát biểu mục tiêu

Xây dựng một hệ thống trong đó **LLM (kèm verifier không phải solver) đảm
nhận trực tiếp việc suy luận ràng buộc nghiệp vụ học vụ tiếng Việt** — gồm
gợi ý môn theo chương trình, sinh thời khoá biểu cá nhân hoá, hiệu chỉnh
theo yêu cầu tự nhiên — **không gọi tới constraint solver chuyên dụng** trong
luồng suy luận chính.

Nhằm trả lời câu hỏi nghiên cứu:

> **RQ-AI**: Liệu một LLM hiện đại, được hỗ trợ bởi verifier nhẹ và chiến
> lược inference phù hợp (CoT, self-verification, best-of-N, RAG), có thể
> đạt chất lượng kết quả **so sánh được với** hoặc **bổ sung cho** hệ thống
> dựa-trên-solver trong miền tổ chức đào tạo đại học tiếng Việt hay không?

### 4.2. Định nghĩa hai khái niệm phân biệt

| Khái niệm  | Định nghĩa                                                                                            | Vai trò trong γ          |
| ---------- | ----------------------------------------------------------------------------------------------------- | ------------------------ |
| **Solver** | Thuật toán tìm kiếm trên không gian lời giải để **tạo ra** lời giải thoả ràng buộc (CP-SAT, MILP, SAT). | **CẤM** dùng trong luồng suy luận chính của γ. |
| **Verifier** | Hàm thuần (pure function) kiểm tra: cho 1 lời giải đầu vào, các ràng buộc có thoả không. Độ phức tạp O(số ràng buộc). | **CÓ THỂ** dùng trong γ để filter / rerank / loop. |

> Ranh giới này là **ràng buộc thiết kế cứng** cho khung thí nghiệm γ. Khi
> nghi ngờ một thành phần là solver hay verifier, ghi vào ADR và quay lại
> mục này để xét.

### 4.3. Lý do chọn C thay vì A, B

| Lý do                     | Diễn giải                                                                                                            |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| Tính mới khoa học rõ ràng | Pure LLM cho constraint reasoning trong domain academic Việt Nam **chưa có công bố tương đương**.                    |
| Tài nguyên khả thi        | Không cần pre-train; backbone API có thể bắt đầu ngay (xem §6).                                                      |
| Trùng khớp trực giác đã xác nhận | LLM xuất sắc ở **tiêu chí mềm khó hình thức hoá** (sở thích sinh viên, cân tải phòng, "thời khoá biểu có hạt sạn để khả thi"). |
| Có không gian ablation thật | γ₀ → γ₅ tạo cây thí nghiệm phong phú (xem §5.4).                                                                     |
| Bộ B (dataset) trở thành sản phẩm phụ tự nhiên | Để đánh giá γ phải có dataset chuẩn → đóng góp B nhận được "miễn phí".                                                |

### 4.4. Phạm vi của hướng C trong khuôn khổ luận văn

- Năng lực AI **được nghiên cứu sâu**: gợi ý môn + sinh thời khoá biểu + hiệu
  chỉnh theo yêu cầu tự nhiên (gọi chung: _Học vụ Personalized Scheduling_).
- Các năng lực AI khác trong CLAUDE.md §1.4 (cảnh báo bất thường, dự đoán
  rớt môn, sinh báo cáo, phân tích cảm xúc feedback, ...) **không** thuộc
  phạm vi RQ-AI — chúng vẫn được triển khai nhưng theo cách _hệ thống có AI_
  thực dụng (LLM + RAG + người duyệt), không vào benchmark Pure LLM.

---

## 5. Khung thí nghiệm so sánh (3-way: α / β / γ)

### 5.1. Định nghĩa ba hệ

| Hệ  | Tên đầy đủ                              | Suy luận ràng buộc do ai làm                                                | LLM dùng vào việc gì                                                   |
| --- | --------------------------------------- | --------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| α   | Pure Solver baseline                    | Constraint solver (OR-Tools CP-SAT).                                        | Không có. UI là form bộ lọc cổ điển.                                   |
| β   | LLM → Tool → Solver (hybrid)            | Solver, nhưng LLM trích yêu cầu / dịch ngôn ngữ tự nhiên thành ràng buộc.   | Front-end ngôn ngữ; **không** suy luận lời giải.                       |
| γ   | Pure LLM (kèm verifier)                 | **LLM**, có thể loop với verifier để self-correct.                          | Suy luận, sinh lời giải, hiệu chỉnh, giải thích.                       |

### 5.2. Giả thuyết cần kiểm chứng

| Mã giả thuyết | Phát biểu                                                                                             |
| ------------- | ----------------------------------------------------------------------------------------------------- |
| H1            | γ đạt **Schedule Validity Rate (SVR)** _so sánh được_ với α trên tập constraint hình thức hoá được. |
| H2            | γ **vượt** β về _user-perceived quality_ trên các tiêu chí mềm (mức "vừa khít" với sở thích).         |
| H3            | γ **không vượt quá** ngưỡng chi phí token / độ trễ chấp nhận được cho ứng dụng học vụ thực tế.        |

Một giả thuyết được _đậu_ khi p-value < 0.05 trên kiểm định phù hợp (xem §7.2).

### 5.3. Chỉ số đo

| Mã chỉ số | Tên                              | Đo cái gì                                                              | Đơn vị      | Ngưỡng kỳ vọng cho γ |
| --------- | -------------------------------- | ---------------------------------------------------------------------- | ----------- | -------------------- |
| M1        | SVR — Schedule Validity Rate     | Tỉ lệ lịch sinh ra **không vi phạm ràng buộc cứng** (do verifier check). | %           | ≥ 0.90 × SVR(α)      |
| M2        | CCR — Constraint Capture Rate    | Tỉ lệ ràng buộc người dùng phát biểu được hệ **hiểu đúng**.            | %           | ≥ 0.85               |
| M3        | UPS — User Preference Score      | Đánh giá chủ quan của sinh viên trên thang 1-5 (human study).         | điểm 1-5    | ≥ 3.8 trung bình     |
| M4        | Latency p95                      | Thời gian phản hồi cuối luồng.                                          | giây        | ≤ 8s                 |
| M5        | Token Cost                       | Trung bình token in + out / lượt.                                       | token       | ≤ 8000               |
| M6        | Soft-criteria adherence          | Mức đạt tiêu chí mềm (tránh tiết sáng sớm, cân tải tuần, …).            | điểm 0-1    | ≥ 0.75               |

Chi tiết đo & gold set: thuộc `08-test-plan-acceptance-criteria.md`. Mục
này chỉ chốt **chỉ số nào** sẽ đo.

### 5.4. Cây ablation cho γ

| Mã   | Cấu hình γ                                                                       | Mục đích                                                          |
| ---- | -------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| γ₀   | LLM thô + prompt cơ bản                                                          | Baseline thấp nhất, đo "LLM raw có làm được không".               |
| γ₁   | + CoT (Chain-of-Thought)                                                         | Đo lợi ích của suy luận có dẫn dắt.                               |
| γ₂   | + RAG (quy chế, chương trình đào tạo, lịch sử SV)                                | Đo lợi ích của tri thức domain.                                   |
| γ₃   | + Verifier loop (LLM sinh → verifier check → LLM sửa)                            | Đo lợi ích của self-correction có ground truth.                   |
| γ₄   | + Best-of-N + Self-Consistency                                                   | Đo lợi ích của lấy mẫu rộng.                                      |
| γ₅   | **Stretch**: γ₃ + LoRA fine-tune trên Vietnamese Academic Constraint dataset.    | Đo: specialization có thắng generalization trên domain này không. |

> γ₅ là **stretch goal**, không bắt buộc. Chỉ thực hiện khi γ₀–γ₄ đã chạy
> ổn định và còn thời gian. Xem [`02-ke-hoach-chuan-bi-nghien-cuu.md §5`](02-ke-hoach-chuan-bi-nghien-cuu.md).

---

## 6. LLM Backbone

### 6.1. Quyết định

**LLM backbone PHẢI dùng API của model có sẵn**, không pre-train, không
fine-tune ở giai đoạn chính.

### 6.2. Yêu cầu cụ thể về backbone

- **PHẢI** sẵn sàng **ít nhất 2 nhà cung cấp khác nhau** (cross-vendor) để:
  - Loại trừ kết luận phụ thuộc một mô hình cụ thể.
  - Giảm rủi ro vendor lock-in (đổi giá, đổi API, ngừng dịch vụ).
- **NÊN** chọn 1 model "_flagship_" (Opus / GPT-4 / Gemini Pro) và 1 model
  "_efficient_" (Haiku / GPT-4o-mini / Gemini Flash) để ablation theo
  trục _capability vs cost_.
- **KHÔNG ĐƯỢC** gọi model qua tài khoản cá nhân không kiểm soát — phải có
  budget tracking và rate-limit rõ ràng (xem [`02-ke-hoach-chuan-bi-nghien-cuu.md §3.3`](02-ke-hoach-chuan-bi-nghien-cuu.md#33-api-llm-backbone)).

### 6.3. Lựa chọn cụ thể (sẽ chốt khi viết SRS)

Hai cặp ứng viên đang ở trạng thái cân nhắc:

| Cặp | Flagship              | Efficient                   | Ưu                                            | Nhược                                                       |
| --- | --------------------- | --------------------------- | --------------------------------------------- | ----------------------------------------------------------- |
| P1  | Claude Opus 4.x       | Claude Haiku 4.5            | Tốt cho long-context, reasoning, hệ sinh thái Anthropic. | Cost cao hơn cho lớp flagship.                              |
| P2  | GPT-4o                | GPT-4o-mini                 | Latency thấp, cost thấp, ecosystem rộng.       | Hạn chế reasoning ở tasks dài.                              |

Bộ đôi từ **các provider khác nhau** (ví dụ Claude Opus + GPT-4o-mini) cũng
là một lựa chọn nếu mục tiêu nhấn cross-vendor robustness.

> Quyết định cuối cùng được ghi nhận trong `10-architecture-decision-record.md`
> dưới mã `ADR-001` (placeholder) khi viết SRS.

### 6.4. Vì sao không pre-train (đường A.1)

Lý do tài nguyên — đã đối chiếu với số liệu công khai 2024-2025:

| Chiều           | Mức tối thiểu để có "LLM dùng được" | Chi phí ước tính         |
| --------------- | ----------------------------------- | ------------------------ |
| Tham số         | 1B+                                 | —                        |
| Token huấn luyện | 300B – 1.5T token sạch              | —                        |
| Compute         | 64–256 × A100/H100 trong 2-6 tháng  | $200K – $5M              |
| Dữ liệu VN sạch | ≥ 50-100 GB text đã lọc             | Cần >1 năm chỉ để gom    |

→ Vượt cấp tài nguyên của luận văn ít nhất 2 bậc độ lớn. **Không khả thi.**

### 6.5. Vì sao fine-tune không phải lựa chọn chính (đường A.2)

Fine-tune (LoRA/QLoRA trên Qwen/Vistral/PhoGPT 7B) **khả thi về compute** (1
GPU, vài ngày), nhưng:

- Cộng thêm 2-4 tháng vào scope (đặc biệt là chất lượng dataset cao hơn
  để train so với để eval).
- Chỉ có giá trị khoa học khi muốn trả lời câu hỏi _specialization vs
  generalization_ — đây là câu hỏi phụ, không phải RQ-AI.

→ Fine-tune được giữ làm **γ₅ stretch goal**, không nằm trên đường găng.

---

## 7. Phương pháp đánh giá

### 7.1. Bộ dữ liệu đánh giá (gold set)

- **Tên gọi**: VACS — _Vietnamese Academic Constraint Set_.
- **Quy mô tối thiểu**: 300 examples ở MVP, mở rộng tới 500 trong giai đoạn
  benchmark.
- **Cấu trúc** từng example:
  ```
  (input_natural_language_request, structured_constraints, gold_solution(s), soft_criteria_rubric)
  ```
- **Phân tầng**: 3 mức độ khó (easy / medium / hard) theo số ràng buộc và
  mức độ ngầm định.
- **Quyền sở hữu**: dataset là sản phẩm phụ của dự án, được công bố theo
  giấy phép phù hợp khi đủ điều kiện (sẽ quyết trong ADR).
- Chi tiết quy trình annotate: [`02-ke-hoach-chuan-bi-nghien-cuu.md §3.1`](02-ke-hoach-chuan-bi-nghien-cuu.md#31-dataset-vacs).

### 7.2. Phương pháp thống kê

- Mỗi cấu hình (γ₀…γ₅, α, β) chạy ≥ 3 lần độc lập trên cùng VACS để có
  bootstrap CI.
- Kiểm định so sánh đôi: **paired t-test** cho SVR / CCR; **Wilcoxon signed-rank**
  cho dữ liệu UPS (Likert).
- Báo cáo: trung bình ± 95% CI; cỡ tác động (Cohen's d) khi p < 0.05.
- Inter-rater agreement cho annotation: **Cohen's κ ≥ 0.7** trước khi
  một bộ con của VACS được đưa vào gold set.

### 7.3. Human study (cho M3 — UPS)

- Thiết kế **within-subject**: mỗi người tham gia thấy lịch từ α, β, γ
  (ẩn nguồn, random thứ tự).
- Cỡ mẫu mục tiêu: **n ≥ 30 sinh viên** (để có power ≈ 0.8 với d = 0.5).
- Quy trình & biểu mẫu: [`02-ke-hoach-chuan-bi-nghien-cuu.md §3.5`](02-ke-hoach-chuan-bi-nghien-cuu.md#35-human-study).

---

## 8. Ranh giới giữa nghiên cứu và nghiệp vụ

| Câu hỏi                                                              | Trả lời                                                                                            |
| -------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| RQ-AI có là điều kiện cần để hệ thống nghiệp vụ hoạt động không?     | **Không.** Hệ thống nghiệp vụ có thể chạy trên α (pure solver) — đây là cấu hình "production-safe". |
| Nếu γ thua α trên SVR, hệ thống nghiệp vụ vẫn dùng cái gì?           | α. γ chỉ được bật cho năng lực _Personalized Scheduling_ ở chế độ thử nghiệm có người duyệt.       |
| Có triển khai γ ra ngoài người dùng thật không?                      | **Có, nhưng chỉ với người duyệt.** Đầu ra γ phải qua bước duyệt (CLAUDE.md §3.4) như mọi AI khác. |
| Năng lực AI nào _bắt buộc_ phải có để dự án thành công về mặt nghiệp vụ? | Các năng lực ngoài Personalized Scheduling — đã liệt kê trong CLAUDE.md §1.4 — vẫn theo lối "có AI thực dụng" với LLM + RAG + người duyệt. |

> **Kết luận:** Hướng C là một **nhánh nghiên cứu cộng thêm** trên hệ thống
> đã tự thân hoàn chỉnh — không phải lý do tồn tại của hệ thống. Nếu RQ-AI
> không trả được, hệ thống vẫn ship.

---

## 9. Rủi ro & giả định

### 9.1. Rủi ro chính

| Mã        | Rủi ro                                                                              | Mức   | Cách giảm thiểu                                                            |
| --------- | ----------------------------------------------------------------------------------- | ----- | -------------------------------------------------------------------------- |
| `RISK-001` | γ không đạt ngưỡng SVR ≥ 0.90 × α → giả thuyết H1 bị bác bỏ.                       | Trung bình | Báo cáo kết quả tiêu cực **vẫn là đóng góp khoa học**; pivot sang RQ phụ về _khi nào_ γ thắng/thua. |
| `RISK-002` | Chi phí token vượt budget khi chạy γ₄ (Best-of-N) trên 500 examples × 3 lần.        | Trung bình | Giới hạn N ≤ 8 cho ablation; chạy γ₄ chỉ ở subset 100 examples đại diện.   |
| `RISK-003` | API provider đổi giá / đổi hành vi giữa giai đoạn benchmark.                       | Thấp  | Yêu cầu 2 provider (§6.2); pin version model trong mỗi run.                |
| `RISK-004` | Dataset VACS bị thiên lệch về một loại ràng buộc → SVR cao "giả".                  | Cao   | Phân tầng 3 mức khó; review chéo annotator; báo cáo SVR theo từng tầng riêng. |
| `RISK-005` | Human study không đủ n = 30 SV tham gia trong thời gian cho phép.                  | Trung bình | Hợp tác sớm với cố vấn học tập để mời SV; có quà cảm ơn / điểm rèn luyện. |
| `RISK-006` | γ₅ (fine-tune) thâm hụt thời gian, kéo cả luận văn trễ hạn.                         | Trung bình | γ₅ là stretch goal; chốt mốc bỏ γ₅ nếu γ₀–γ₄ chưa xong tại thời điểm cut-off. |

### 9.2. Giả định

- API provider hiện hành giữ chính sách giá và quota ổn định trong vòng 6
  tháng từ ngày tạo tài liệu.
- Có thể tiếp cận quy chế đào tạo + chương trình đào tạo của ≥ 1 khoa/trường
  để dùng làm corpus RAG.
- Đề tài luận văn cho phép cấu hình thí nghiệm với người dùng thật (sinh
  viên) cho human study.

> Nếu **bất kỳ giả định nào sai**, tài liệu này phải được cập nhật và bump
> theo §11 quy chuẩn.

---

## 10. Tham chiếu

- [`../../CLAUDE.md`](../../CLAUDE.md) — phạm vi & ranh giới dự án (§1.4 nhóm bài toán AI; §3.4 ràng buộc AI).
- [`../SDLC/00-quy-chuan.md`](../SDLC/00-quy-chuan.md) — quy chuẩn tài liệu (đặc biệt §6 RFC 2119, §12.3 đo lường).
- [`../agents.md`](../agents.md) — chỉ dẫn cho agent khi soạn SDLC.
- [`02-ke-hoach-chuan-bi-nghien-cuu.md`](02-ke-hoach-chuan-bi-nghien-cuu.md) — kế hoạch chuẩn bị & lộ trình.
- Wei et al. 2022 — _Chain-of-Thought Prompting Elicits Reasoning in LLMs_.
- Lewis et al. 2020 — _Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks_.
- Wang et al. 2023 — _Self-Consistency Improves Chain of Thought Reasoning_.
- Yao et al. 2023 — _ReAct: Synergizing Reasoning and Acting in LLMs_.
- Cohen 1988 — _Statistical Power Analysis for the Behavioral Sciences_ (cỡ tác động).
