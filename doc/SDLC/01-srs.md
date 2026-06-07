# 01 — Đặc tả Yêu cầu Phần mềm (Software Requirements Specification)

## 1. Thông tin tài liệu

### 1.1. Metadata

| Thuộc tính         | Giá trị                                                                                                                                                                                                     |
| :----------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Tên tài liệu       | Đặc tả Yêu cầu Phần mềm (_Software Requirements Specification_)                                                                                                                                            |
| Mã tài liệu        | 01-srs                                                                                                                                                                                                      |
| Dự án              | Nền tảng tích hợp hỗ trợ tổ chức đào tạo (NCKH_1)                                                                                                                                                          |
| Phiên bản          | v0.2.0                                                                                                                                                                                                      |
| Trạng thái         | Draft                                                                                                                                                                                                       |
| Người viết         | Hiếu                                                                                                                                                                                          |
| Người duyệt        | (chưa duyệt)                                                                                                                                                                                                |
| Ngày tạo           | 2026-06-02                                                                                                                                                                                                  |
| Ngày cập nhật      | 2026-06-03                                                                                                                                                                                                  |
| Tài liệu liên quan | [`../../CLAUDE.md`](../../CLAUDE.md), [`../SDLC/00-quy-chuan.md`](00-quy-chuan.md), [`../context/01-muc-tieu-nghien-cuu-ai.md`](../context/01-muc-tieu-nghien-cuu-ai.md), [`../context/02-ke-hoach-chuan-bi-nghien-cuu.md`](../context/02-ke-hoach-chuan-bi-nghien-cuu.md) |

### 1.2. Lịch sử thay đổi (Changelog)

| Phiên bản | Ngày       | Tác giả           | Thay đổi                          |
| :-------- | :--------- | :---------------- | :-------------------------------- |
| 0.2.0     | 2026-06-03 | team-architecture | Thêm FR-019–FR-024 (học kỳ, phòng, phân công GV, đề thi, xuất báo cáo, audit log); thêm NFR-012–NFR-015 (log retention, test coverage, scaling, rate limit); bổ sung luồng ngoại lệ UC-003, UC-013; mở rộng glossary 8 thuật ngữ; cập nhật ma trận truy ngược thêm cột NFR. |
| 0.1.0     | 2026-06-02 | team-architecture | Bản nháp đầu tiên — đầy đủ UC, FR, NFR, AI, ràng buộc, glossary, traceability. |

---

## 2. Giới thiệu

### 2.1. Mục đích

Tài liệu này đặc tả toàn bộ yêu cầu phần mềm của **Nền tảng tích hợp hỗ trợ tổ chức đào tạo đại học có ứng dụng AI** (mã nội bộ: _NCKH_1_). Tài liệu là đầu vào chính cho [`02-hld.md`](02-hld.md) (thiết kế tổng thể) và [`08-test-plan-acceptance-criteria.md`](08-test-plan-acceptance-criteria.md) (kiểm thử & nghiệm thu).

### 2.2. Phạm vi

Nền tảng phục vụ **sáu nhóm người dùng nghiệp vụ** và **một vai trò kỹ thuật** trong phạm vi một khoa/trường đại học, trải trên **tám nhóm bài toán con** từ lập kế hoạch học tập, tổ chức đào tạo, tài chính học vụ, đến khảo thí, đánh giá giảng dạy và báo cáo lãnh đạo. AI được ứng dụng xuyên suốt theo hướng nghiên cứu Pure LLM end-to-end (xem [`doc/context/01-muc-tieu-nghien-cuu-ai.md`](../context/01-muc-tieu-nghien-cuu-ai.md)).

### 2.3. Định nghĩa từ khóa nghĩa vụ

Tài liệu này tuân theo RFC 2119 / RFC 8174 ([`00-quy-chuan.md §6`](00-quy-chuan.md#6-mức-độ-nghĩa-vụ-rfc-2119--rfc-8174)). Các từ khoá **PHẢI**, **KHÔNG ĐƯỢC**, **NÊN**, **KHÔNG NÊN**, **CÓ THỂ** được dùng đúng nghĩa kỹ thuật và viết hoa trong toàn tài liệu.

### 2.4. Tài liệu tham chiếu

Xem [mục Tham chiếu](#10-tham-chiếu) ở cuối tài liệu.

---

## 3. Bối cảnh & các bên liên quan

### 3.1. Vấn đề cần giải quyết

Công tác tổ chức đào tạo tại một cơ sở giáo dục đại học là quy trình nhiều bên liên quan, ràng buộc lẫn nhau: khoa/bộ môn xây dựng chương trình và mở _học phần_ (_course_), phòng đào tạo tổ chức _lớp học phần_ (_course offering_) và quản lý học kỳ, giảng viên được phân công giảng dạy, sinh viên đăng ký học theo chương trình của mình. Xen suốt là các hoạt động tài chính (học phí) và trao đổi thông tin (thông báo, hỏi đáp học vụ).

Hiện trạng: các công cụ hỗ trợ rời rạc, thủ công và thiếu thông minh; khối lượng lớn công việc lặp lại và mang tính phân tích/giao tiếp chưa được tự động hoá.

### 3.2. Các bên liên quan

| Nhóm                      | Vai trò chính                                                                                                                                                                              | Mức quan tâm |
| :------------------------ | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------- |
| Sinh viên                 | Lập kế hoạch học tập, đăng ký học phần, xem học phí, hỏi đáp học vụ, khảo sát đánh giá giảng dạy.                                                                                        | Cao          |
| Phòng Đào tạo             | Tổ chức lớp học phần, theo dõi học phí, giám sát bất thường, tổng hợp đánh giá giảng dạy.                                                                                                 | Cao          |
| Khoa / Bộ môn             | Quản lý chương trình, học phần, phân công giảng dạy, theo dõi chất lượng giảng dạy.                                                                                                       | Cao          |
| Giảng viên                | Xem lớp được phân công, lịch dạy, danh sách sinh viên, xem feedback đánh giá. Có thể kiêm Cố vấn học tập (CVHT): duyệt kế hoạch và theo dõi tiến độ sinh viên.                            | Cao          |
| Phòng Khảo thí            | Lập lịch thi, phân phòng, phân giám thị, quản lý đề thi.                                                                                                                                  | Cao          |
| Lãnh đạo cấp trường       | Đọc dashboard tổng hợp & báo cáo (chỉ đọc — _read-only_). Không cấu hình hệ thống, không nhập/sửa dữ liệu nghiệp vụ.                                                                     | Trung bình   |
| Quản trị viên hệ thống    | Vai trò kỹ thuật: quản tài khoản, phân vai trò, backup, log. Không tác động lên dữ liệu nghiệp vụ.                                                                                        | Trung bình   |

---

## 4. Đặc tả Use Case

> Quy ước: mỗi _use case_ (ca sử dụng) có ID `UC-XXX`, actor chính, tiền điều kiện, luồng chính, luồng ngoại lệ và hậu điều kiện. Luồng ngoại lệ chỉ liệt kê trường hợp khác biệt đáng kể so với luồng chính.

### 4.1. Nhóm UC: Kế hoạch học tập sinh viên

---

#### UC-001 — Gợi ý môn học theo chương trình

<a id="uc-001"></a>

| Thuộc tính       | Giá trị                                                                  |
| :--------------- | :----------------------------------------------------------------------- |
| Actor chính      | Sinh viên                                                                |
| Actor phụ        | Hệ thống AI (γ/α), Cố vấn học tập (CVHT)                                 |
| Tiền điều kiện   | Sinh viên đã đăng nhập; dữ liệu chương trình đào tạo và kết quả học tập sẵn có. |
| Hậu điều kiện    | Sinh viên nhận danh sách môn học được gợi ý kèm lý do; có thể lưu vào kế hoạch nháp. |

**Luồng chính:**

1. Sinh viên mở màn hình lập kế hoạch học kỳ.
2. Hệ thống lấy dữ liệu: chương trình đào tạo, môn đã hoàn thành, môn đang học, số tín chỉ tích luỹ.
3. AI phân tích điều kiện tiên quyết, tiến độ còn lại, khả năng xung đột lịch (sơ bộ).
4. Hệ thống hiển thị danh sách môn gợi ý, xếp theo độ ưu tiên, kèm lý do ngắn gọn (môn tiên quyết cho kỳ sau, môn sắp khoá lớp, ...).
5. Sinh viên chọn một hoặc nhiều môn để thêm vào kế hoạch nháp.
6. Hệ thống lưu kế hoạch nháp, chưa gửi CVHT.

**Luồng ngoại lệ:**

- *4a.* Không tìm thấy môn nào phù hợp điều kiện → hệ thống hiển thị thông báo và giải thích lý do; gợi ý liên hệ CVHT.
- *3a.* AI không phản hồi trong ngưỡng thời gian → hệ thống fallback sang gợi ý dựa quy tắc đơn giản (không AI), ghi log.

---

#### UC-002 — Sinh thời khoá biểu cá nhân hoá

<a id="uc-002"></a>

| Thuộc tính       | Giá trị                                                                              |
| :--------------- | :----------------------------------------------------------------------------------- |
| Actor chính      | Sinh viên                                                                            |
| Actor phụ        | Hệ thống AI (γ/α/β), Verifier                                                        |
| Tiền điều kiện   | Sinh viên có danh sách môn muốn học trong kỳ; dữ liệu lớp học phần và thời khoá biểu đã được phòng đào tạo công bố. |
| Hậu điều kiện    | Sinh viên nhận phương án thời khoá biểu không trùng lịch, kèm điểm chất lượng mềm. |

**Luồng chính:**

1. Sinh viên nhập yêu cầu bằng ngôn ngữ tự nhiên hoặc chọn qua bộ lọc (tránh thứ mấy, không học sau giờ nào, số tín chỉ mong muốn, ...).
2. Hệ thống chuẩn bị ngữ cảnh: danh sách lớp học phần mở, sở thích và ràng buộc cứng của sinh viên.
3. AI (γ) suy luận và sinh ra ≥ 1 phương án thời khoá biểu.
4. Verifier kiểm tra từng phương án: không vi phạm ràng buộc cứng (trùng tiết, tiên quyết, giới hạn tín chỉ, lớp còn chỗ).
5. Hệ thống hiển thị phương án hợp lệ, xếp theo điểm chất lượng mềm, kèm giải thích ngắn.
6. Sinh viên chọn một phương án hoặc yêu cầu hiệu chỉnh.

**Luồng ngoại lệ:**

- *4a.* Không phương án nào vượt qua verifier sau N lần thử → hệ thống thông báo không tìm được lịch hợp lệ với ràng buộc đã cho; đề xuất nới lỏng ràng buộc mềm.
- *1a.* Sinh viên không nhập yêu cầu → hệ thống dùng ràng buộc mặc định (không có yêu cầu mềm thêm).

---

#### UC-003 — Duyệt kế hoạch học tập (CVHT)

<a id="uc-003"></a>

| Thuộc tính       | Giá trị                                                                              |
| :--------------- | :----------------------------------------------------------------------------------- |
| Actor chính      | Cố vấn học tập (CVHT — giảng viên có thêm capability này)                            |
| Actor phụ        | Sinh viên                                                                            |
| Tiền điều kiện   | Sinh viên đã gửi kế hoạch học tập nháp; CVHT được phân công cho sinh viên đó.       |
| Hậu điều kiện    | Kế hoạch được duyệt hoặc trả về với nhận xét; sinh viên nhận thông báo.             |

**Luồng chính:**

1. CVHT xem danh sách kế hoạch chờ duyệt của sinh viên được phân công.
2. CVHT mở kế hoạch của từng sinh viên, xem danh sách môn, tiến độ tích luỹ, cảnh báo (nếu có) do hệ thống tạo.
3. CVHT duyệt (chấp thuận) hoặc gửi nhận xét yêu cầu sinh viên chỉnh lại.
4. Hệ thống ghi nhận trạng thái kế hoạch; gửi thông báo cho sinh viên.

**Luồng ngoại lệ:**

- *1a.* Không có kế hoạch nào chờ duyệt → hệ thống hiển thị trang trống, thông báo "Không có kế hoạch chờ duyệt".
- *3a.* CVHT gửi nhận xét → sinh viên chỉnh sửa và gửi lại; CVHT nhận thông báo mới; lặp lại từ bước 2.

---

### 4.2. Nhóm UC: Tổ chức & giám sát đào tạo

---

#### UC-004 — Quản lý lớp học phần

<a id="uc-004"></a>

| Thuộc tính       | Giá trị                                                                    |
| :--------------- | :------------------------------------------------------------------------- |
| Actor chính      | Phòng Đào tạo                                                              |
| Tiền điều kiện   | Học phần đã có trong danh mục; học kỳ đã được tạo.                         |
| Hậu điều kiện    | Lớp học phần được tạo/cập nhật với đầy đủ thông tin; hiển thị cho đăng ký. |

**Luồng chính:**

1. Phòng Đào tạo tạo lớp học phần mới: chọn học phần, giảng viên, sĩ số tối đa, phòng học, lịch dạy.
2. Hệ thống kiểm tra xung đột: giảng viên đã có lớp khác cùng tiết, phòng đã dùng cùng tiết.
3. Nếu không có xung đột, lớp được lưu với trạng thái `active`.
4. Phòng Đào tạo có thể cập nhật (đổi phòng, đổi giảng viên) hoặc huỷ lớp trước ngày đăng ký kết thúc.

**Luồng ngoại lệ:**

- *2a.* Phát hiện xung đột → hệ thống hiển thị chi tiết xung đột, không lưu, yêu cầu sửa.

---

#### UC-005 — Cảnh báo bất thường đào tạo

<a id="uc-005"></a>

| Thuộc tính       | Giá trị                                                              |
| :--------------- | :------------------------------------------------------------------- |
| Actor chính      | Hệ thống (tự động), Phòng Đào tạo                                    |
| Tiền điều kiện   | Dữ liệu lớp học phần, đăng ký, kết quả học tập đã có trong hệ thống. |
| Hậu điều kiện    | Cảnh báo được ghi log và hiển thị trên dashboard Phòng Đào tạo.     |

**Luồng chính:**

1. Hệ thống chạy job định kỳ (hoặc theo sự kiện) kiểm tra: lớp quá tải (đăng ký > sĩ số), lớp thiếu sinh viên (< ngưỡng mở), giảng viên chưa được phân công trước hạn.
2. AI phân tích xu hướng: môn học có tỉ lệ rớt cao bất thường, sinh viên có nguy cơ học vụ.
3. Hệ thống tạo cảnh báo với mức độ (thấp/trung bình/cao), ghi log.
4. Phòng Đào tạo xem và xử lý cảnh báo.

---

#### UC-006 — Đăng ký học phần (sinh viên)

<a id="uc-006"></a>

| Thuộc tính       | Giá trị                                                                      |
| :--------------- | :--------------------------------------------------------------------------- |
| Actor chính      | Sinh viên                                                                    |
| Tiền điều kiện   | Đang trong thời gian đăng ký học; lớp học phần đang mở; sinh viên đã lập kế hoạch (tuỳ chọn). |
| Hậu điều kiện    | Sinh viên được ghi danh vào lớp học phần; chỗ trống giảm 1; học phí dự kiến được cập nhật. |

**Luồng chính:**

1. Sinh viên tìm và chọn lớp học phần muốn đăng ký.
2. Hệ thống kiểm tra: còn chỗ, không trùng lịch với lớp đã đăng ký, đủ điều kiện tiên quyết, không vượt giới hạn tín chỉ/kỳ.
3. Hệ thống ghi danh sinh viên, tạo bản ghi tài chính dự kiến.
4. Sinh viên nhận xác nhận.

**Luồng ngoại lệ:**

- *2a.* Lớp hết chỗ → hệ thống thông báo; sinh viên có thể vào danh sách chờ.
- *2b.* Vi phạm điều kiện tiên quyết → hệ thống giải thích cụ thể môn nào còn thiếu.
- *2c.* Trùng lịch → hệ thống chỉ rõ lớp bị trùng.

---

### 4.3. Nhóm UC: Tài chính học vụ (sandbox)

---

#### UC-007 — Xem và ước tính học phí

<a id="uc-007"></a>

| Thuộc tính       | Giá trị                                                                    |
| :--------------- | :------------------------------------------------------------------------- |
| Actor chính      | Sinh viên                                                                  |
| Tiền điều kiện   | Sinh viên đã đăng nhập; có ít nhất 1 lớp học phần đã đăng ký hoặc trong kế hoạch. |
| Hậu điều kiện    | Sinh viên thấy học phí hiện tại và dự kiến. Không có giao dịch tiền thật. |

**Luồng chính:**

1. Sinh viên mở màn hình học phí.
2. Hệ thống tính và hiển thị: học phí kỳ hiện tại theo tín chỉ đã đăng ký, trạng thái thanh toán (môi trường sandbox), dự kiến học phí nếu đăng ký thêm.
3. Sinh viên có thể thực hiện thanh toán mô phỏng (sandbox); giao dịch không ảnh hưởng tiền thật.

---

#### UC-008 — Theo dõi học phí và cảnh báo công nợ (Phòng Đào tạo)

<a id="uc-008"></a>

| Thuộc tính       | Giá trị                                                                   |
| :--------------- | :------------------------------------------------------------------------ |
| Actor chính      | Phòng Đào tạo                                                             |
| Tiền điều kiện   | Dữ liệu học phí và trạng thái thanh toán đã có trong hệ thống (sandbox). |
| Hậu điều kiện    | Phòng Đào tạo thấy báo cáo tổng hợp; sinh viên có công nợ được đánh dấu. |

**Luồng chính:**

1. Phòng Đào tạo mở màn hình theo dõi học phí.
2. Hệ thống hiển thị: tổng thu kỳ (sandbox), danh sách sinh viên chưa thanh toán, sinh viên sắp đến hạn.
3. AI tạo cảnh báo cho sinh viên có nguy cơ nợ học phí quá hạn (dựa lịch sử).
4. Phòng Đào tạo có thể xuất báo cáo hoặc gửi thông báo nhắc nhở (qua bước duyệt).

---

### 4.4. Nhóm UC: Quản lý kết quả học tập

---

#### UC-009 — Nhập và công bố điểm

<a id="uc-009"></a>

| Thuộc tính       | Giá trị                                                                             |
| :--------------- | :---------------------------------------------------------------------------------- |
| Actor chính      | Giảng viên                                                                          |
| Actor phụ        | Phòng Đào tạo (duyệt), Khoa (giám sát)                                              |
| Tiền điều kiện   | Lớp học phần đã kết thúc; giảng viên được phân công lớp đó.                         |
| Hậu điều kiện    | Điểm được lưu, duyệt, công bố; sinh viên nhận thông báo; bảng điểm tích luỹ cập nhật. |

**Luồng chính:**

1. Giảng viên nhập điểm cho từng sinh viên của lớp.
2. Phòng Đào tạo hoặc Khoa duyệt điểm.
3. Hệ thống công bố điểm; cập nhật bảng điểm tích luỹ (_transcript_); kiểm tra cảnh báo học vụ (GPA thấp, số tín chỉ không đạt vượt ngưỡng).
4. Sinh viên nhận thông báo và xem điểm.

---

#### UC-010 — Xem bảng điểm tích luỹ

<a id="uc-010"></a>

| Thuộc tính       | Giá trị                                                        |
| :--------------- | :------------------------------------------------------------- |
| Actor chính      | Sinh viên                                                      |
| Tiền điều kiện   | Sinh viên đã đăng nhập; có ít nhất 1 học kỳ với điểm đã công bố. |
| Hậu điều kiện    | Sinh viên xem được bảng điểm đầy đủ.                          |

**Luồng chính:**

1. Sinh viên mở màn hình bảng điểm.
2. Hệ thống hiển thị toàn bộ điểm theo từng học kỳ, GPA tích luỹ, số tín chỉ đã đạt, tình trạng học vụ.
3. AI hiển thị dự báo nguy cơ học vụ nếu có (xem [`AI-005`](#ai-005)).

---

### 4.5. Nhóm UC: Tổ chức khảo thí

---

#### UC-011 — Lập lịch thi

<a id="uc-011"></a>

| Thuộc tính       | Giá trị                                                                        |
| :--------------- | :----------------------------------------------------------------------------- |
| Actor chính      | Phòng Khảo thí                                                                 |
| Actor phụ        | Hệ thống AI (sinh lịch tối ưu)                                                  |
| Tiền điều kiện   | Danh sách lớp học phần kỳ hiện tại đã chốt; danh sách phòng thi đã có.         |
| Hậu điều kiện    | Lịch thi được lưu; không có sinh viên bị trùng lịch thi; phân phòng và giám thị xong. |

**Luồng chính:**

1. Phòng Khảo thí nhập điều kiện: khung thời gian thi, số phòng, sĩ số tối đa mỗi phòng.
2. AI sinh đề xuất lịch thi tối ưu: giảm số sinh viên bị trùng lịch, cân bằng tải phòng, ưu tiên phân bổ đều trong khung thời gian.
3. Phòng Khảo thí xem đề xuất, chỉnh sửa thủ công nếu cần.
4. Phòng Khảo thí xác nhận và công bố lịch thi.

---

#### UC-012 — Phân phòng và giám thị

<a id="uc-012"></a>

| Thuộc tính       | Giá trị                                                              |
| :--------------- | :------------------------------------------------------------------- |
| Actor chính      | Phòng Khảo thí                                                       |
| Tiền điều kiện   | Lịch thi đã được tạo ([`UC-011`](#uc-011)); danh sách giảng viên đủ điều kiện làm giám thị. |
| Hậu điều kiện    | Mỗi ca thi có đủ giám thị; không giám thị nào bị trùng ca.          |

**Luồng chính:**

1. Phòng Khảo thí yêu cầu hệ thống phân giám thị tự động.
2. Hệ thống phân công giám thị theo quy tắc: không phân giảng viên dạy lớp đó làm giám thị lớp đó, cân bằng số ca giám thị giữa các giảng viên.
3. Phòng Khảo thí xem, điều chỉnh, xác nhận.

---

### 4.6. Nhóm UC: Đánh giá giảng dạy

---

#### UC-013 — Khảo sát đánh giá giảng dạy (sinh viên)

<a id="uc-013"></a>

| Thuộc tính       | Giá trị                                                                          |
| :--------------- | :------------------------------------------------------------------------------- |
| Actor chính      | Sinh viên                                                                        |
| Tiền điều kiện   | Đang trong thời gian khảo sát cuối kỳ; sinh viên đã đăng ký lớp học phần đó.    |
| Hậu điều kiện    | Phản hồi được lưu; danh tính không gắn kết với câu trả lời (ẩn danh).           |

**Luồng chính:**

1. Sinh viên nhận thông báo mở khảo sát.
2. Sinh viên điền phiếu đánh giá: phần trắc nghiệm thang Likert + phần nhận xét tự do.
3. Hệ thống lưu kết quả ẩn danh.

**Luồng ngoại lệ:**

- *1a.* Sinh viên đã hoàn thành khảo sát lớp này → hệ thống hiển thị trạng thái "Đã hoàn thành", không cho điền lại.
- *2a.* Sinh viên bỏ dở giữa chừng → hệ thống **KHÔNG** lưu dữ liệu một phần; phiếu chỉ được lưu khi bấm nộp.

---

#### UC-014 — Xem tổng hợp đánh giá giảng dạy

<a id="uc-014"></a>

| Thuộc tính       | Giá trị                                                                                   |
| :--------------- | :---------------------------------------------------------------------------------------- |
| Actor chính      | Giảng viên (xem về mình), Khoa, Phòng Đào tạo, Lãnh đạo                                   |
| Tiền điều kiện   | Kỳ khảo sát đã kết thúc; đủ số lượng phản hồi để công bố (ngưỡng tối thiểu).              |
| Hậu điều kiện    | Các bên liên quan xem được báo cáo phù hợp với quyền của mình.                            |

**Luồng chính:**

1. Actor mở màn hình báo cáo đánh giá.
2. AI tổng hợp: điểm trung bình thang Likert, phân tích cảm xúc nhận xét tự do, phân loại chủ đề (nội dung, phương pháp, tương tác, ...).
3. Hệ thống hiển thị báo cáo theo phạm vi quyền:
   - Giảng viên thấy kết quả lớp mình dạy.
   - Khoa thấy toàn bộ giảng viên trong khoa.
   - Phòng Đào tạo và Lãnh đạo thấy toàn trường.

---

### 4.7. Nhóm UC: Hỏi đáp học vụ (AI)

---

#### UC-015 — Hỏi đáp học vụ có trích dẫn nguồn

<a id="uc-015"></a>

| Thuộc tính       | Giá trị                                                                             |
| :--------------- | :---------------------------------------------------------------------------------- |
| Actor chính      | Sinh viên                                                                           |
| Actor phụ        | Hệ thống AI (RAG)                                                                   |
| Tiền điều kiện   | Corpus quy chế, biểu mẫu, chương trình đào tạo đã được cập nhật vào hệ thống RAG.  |
| Hậu điều kiện    | Sinh viên nhận câu trả lời kèm trích dẫn nguồn cụ thể (điều khoản, trang tài liệu). |

**Luồng chính:**

1. Sinh viên nhập câu hỏi học vụ bằng ngôn ngữ tự nhiên.
2. Hệ thống tìm kiếm tài liệu liên quan trong corpus RAG.
3. AI tổng hợp câu trả lời từ các đoạn tài liệu tìm được; gắn trích dẫn nguồn cho mỗi phát biểu.
4. Hệ thống hiển thị trả lời và danh sách nguồn tham chiếu có thể click.

**Luồng ngoại lệ:**

- *2a.* Không tìm thấy tài liệu liên quan → AI thông báo không có thông tin đủ tin cậy để trả lời; đề xuất liên hệ phòng đào tạo.

---

### 4.8. Nhóm UC: Báo cáo lãnh đạo

---

#### UC-016 — Xem dashboard tổng hợp

<a id="uc-016"></a>

| Thuộc tính       | Giá trị                                                                          |
| :--------------- | :------------------------------------------------------------------------------- |
| Actor chính      | Lãnh đạo cấp trường                                                              |
| Tiền điều kiện   | Đã đăng nhập; có dữ liệu từ ít nhất 1 học kỳ hoàn chỉnh.                         |
| Hậu điều kiện    | Lãnh đạo xem được dashboard chỉ số đào tạo; không được sửa bất kỳ dữ liệu nào.  |

**Luồng chính:**

1. Lãnh đạo mở dashboard.
2. Hệ thống hiển thị: tỉ lệ tốt nghiệp đúng hạn, tải giảng dạy trung bình, hiệu suất chương trình (tỉ lệ hoàn thành môn), xu hướng theo thời gian.
3. AI sinh tóm tắt định kỳ (tuần/tháng) về bất thường đáng chú ý.
4. Lãnh đạo có thể xuất báo cáo PDF; không có thao tác nhập/sửa dữ liệu.

---

## 5. Yêu cầu chức năng

> Mỗi yêu cầu phát biểu theo: "Hệ thống **PHẢI** ..." hoặc "Hệ thống **NÊN** ..." Mỗi FR ánh xạ tới ít nhất 1 UC.

### 5.1. Nhóm FR: Xác thực & phân quyền

---

#### FR-001 — Đăng nhập

<a id="fr-001"></a>

Hệ thống **PHẢI** cho phép người dùng đăng nhập bằng email + mật khẩu. Hệ thống **NÊN** hỗ trợ thêm SSO (_Single Sign-On_) theo cổng xác thực của trường nếu cổng đó tồn tại.

_UC liên quan:_ tất cả UC.

---

#### FR-002 — Phân vai trò

<a id="fr-002"></a>

Hệ thống **PHẢI** gán vai trò cho từng tài khoản: `student`, `lecturer`, `academic_advisor` (khả năng bổ sung trên `lecturer`), `department`, `training_office`, `examination_office`, `leadership`, `sysadmin`. Hệ thống **PHẢI** kiểm tra quyền trước mỗi thao tác thay đổi dữ liệu.

_UC liên quan:_ tất cả UC.

---

#### FR-003 — Quản lý tài khoản (SysAdmin)

<a id="fr-003"></a>

Hệ thống **PHẢI** cho phép SysAdmin tạo, vô hiệu hoá, đổi vai trò tài khoản. Hệ thống **KHÔNG ĐƯỢC** cho SysAdmin đọc hoặc sửa dữ liệu nghiệp vụ (điểm, học phí, kết quả khảo sát).

---

### 5.2. Nhóm FR: Chương trình & học phần

---

#### FR-004 — Quản lý chương trình đào tạo

<a id="fr-004"></a>

Hệ thống **PHẢI** lưu trữ chương trình đào tạo gồm: danh sách học phần, số tín chỉ, học phần tiên quyết, học phần tương đương, phân loại (đại cương/cơ sở ngành/chuyên ngành/tự chọn). Khoa/Bộ môn **PHẢI** được phép tạo và sửa chương trình.

_UC liên quan:_ [`UC-001`](#uc-001), [`UC-002`](#uc-002), [`UC-006`](#uc-006).

---

#### FR-005 — Quản lý học phần

<a id="fr-005"></a>

Hệ thống **PHẢI** cho phép Khoa tạo, sửa, vô hiệu hoá học phần với các thuộc tính: mã học phần, tên, số tín chỉ, mô tả, học phần tiên quyết, số tiết lý thuyết/thực hành.

---

### 5.3. Nhóm FR: Lớp học phần & đăng ký

---

#### FR-006 — Quản lý lớp học phần

<a id="fr-006"></a>

Hệ thống **PHẢI** cho phép Phòng Đào tạo tạo lớp học phần với: học phần, giảng viên, học kỳ, sĩ số tối đa, phòng học, thời khoá biểu (tiết–thứ–tuần). Hệ thống **PHẢI** tự động kiểm tra và từ chối tạo lớp nếu giảng viên hoặc phòng học bị xung đột lịch.

_UC liên quan:_ [`UC-004`](#uc-004).

---

#### FR-007 — Đăng ký học phần

<a id="fr-007"></a>

Hệ thống **PHẢI** cho phép sinh viên đăng ký lớp học phần trong thời gian đăng ký. Hệ thống **PHẢI** kiểm tra và từ chối nếu: lớp hết chỗ, sinh viên chưa đủ tiên quyết, đăng ký gây trùng lịch, vượt giới hạn tín chỉ/kỳ. Hệ thống **PHẢI** xử lý thao tác đăng ký _idempotent_ với `Idempotency-Key` để tránh đăng ký trùng do gửi lại yêu cầu.

_UC liên quan:_ [`UC-006`](#uc-006).

---

#### FR-008 — Danh sách chờ

<a id="fr-008"></a>

Hệ thống **NÊN** cho phép sinh viên đăng ký vào danh sách chờ khi lớp hết chỗ. Khi có chỗ trống, hệ thống **NÊN** thông báo cho sinh viên đầu danh sách.

---

### 5.4. Nhóm FR: Tài chính học vụ (sandbox)

---

#### FR-009 — Tính học phí theo tín chỉ

<a id="fr-009"></a>

Hệ thống **PHẢI** tính học phí dựa trên số tín chỉ đăng ký và đơn giá tín chỉ (cấu hình theo học kỳ/chương trình). Toàn bộ giao dịch học phí **PHẢI** hoạt động trong môi trường sandbox; **KHÔNG ĐƯỢC** lưu hoặc xử lý dữ liệu tài chính thật.

_UC liên quan:_ [`UC-007`](#uc-007), [`UC-008`](#uc-008).

---

#### FR-010 — Thanh toán mô phỏng (sandbox)

<a id="fr-010"></a>

Hệ thống **PHẢI** cung cấp giao diện thanh toán học phí mô phỏng. Hệ thống **KHÔNG ĐƯỢC** tích hợp cổng thanh toán thật. Thao tác thanh toán sandbox **PHẢI** idempotent với `Idempotency-Key`.

---

### 5.5. Nhóm FR: Kết quả học tập

---

#### FR-011 — Nhập điểm

<a id="fr-011"></a>

Hệ thống **PHẢI** cho phép giảng viên nhập điểm cho từng sinh viên trong lớp học phần được phân công. Hệ thống **PHẢI** hỗ trợ nhiều thành phần điểm (chuyên cần, giữa kỳ, cuối kỳ) theo trọng số cấu hình của từng học phần.

_UC liên quan:_ [`UC-009`](#uc-009).

---

#### FR-012 — Duyệt và công bố điểm

<a id="fr-012"></a>

Hệ thống **PHẢI** yêu cầu bước duyệt (Phòng Đào tạo hoặc Khoa) trước khi điểm được công bố cho sinh viên. Hệ thống **PHẢI** cập nhật bảng điểm tích luỹ ngay sau khi điểm được công bố.

---

#### FR-013 — Bảng điểm tích luỹ

<a id="fr-013"></a>

Hệ thống **PHẢI** tính và lưu GPA tích luỹ, số tín chỉ đã đạt, tình trạng học vụ (bình thường / cảnh báo / buộc thôi học) theo quy chế đào tạo.

_UC liên quan:_ [`UC-010`](#uc-010).

---

### 5.6. Nhóm FR: Khảo thí

---

#### FR-014 — Lập lịch thi

<a id="fr-014"></a>

Hệ thống **PHẢI** cho phép Phòng Khảo thí tạo và chỉnh sửa lịch thi. Hệ thống **PHẢI** kiểm tra và cảnh báo nếu có sinh viên bị trùng lịch thi.

_UC liên quan:_ [`UC-011`](#uc-011).

---

#### FR-015 — Phân phòng và giám thị

<a id="fr-015"></a>

Hệ thống **PHẢI** hỗ trợ phân công phòng thi và giám thị tự động. Hệ thống **PHẢI** đảm bảo: sức chứa phòng ≥ số sinh viên, không phân giảng viên dạy lớp đó làm giám thị chính cho lớp đó.

_UC liên quan:_ [`UC-012`](#uc-012).

---

### 5.7. Nhóm FR: Đánh giá giảng dạy

---

#### FR-016 — Khảo sát đánh giá

<a id="fr-016"></a>

Hệ thống **PHẢI** tổ chức khảo sát ẩn danh cuối kỳ cho sinh viên. Hệ thống **KHÔNG ĐƯỢC** lưu thông tin liên kết danh tính sinh viên với câu trả lời khảo sát. Hệ thống **PHẢI** chỉ công bố kết quả khi số lượng phản hồi ≥ ngưỡng tối thiểu (mặc định: 5 phản hồi/lớp, có thể cấu hình).

_UC liên quan:_ [`UC-013`](#uc-013), [`UC-014`](#uc-014).

---

### 5.8. Nhóm FR: Quản lý học kỳ & phòng học

---

#### FR-019 — Quản lý học kỳ

<a id="fr-019"></a>

Hệ thống **PHẢI** cho phép Phòng Đào tạo tạo và quản lý học kỳ với các thuộc tính: mã học kỳ, tên hiển thị, ngày bắt đầu, ngày kết thúc, thời điểm mở/đóng đăng ký, trạng thái (`planning` / `registration_open` / `in_progress` / `completed`). Hệ thống **PHẢI** chỉ cho phép tạo lớp học phần khi học kỳ ở trạng thái `planning` hoặc `in_progress`.

_UC liên quan:_ [`UC-004`](#uc-004), [`UC-006`](#uc-006).

---

#### FR-020 — Quản lý phòng học & phòng thi

<a id="fr-020"></a>

Hệ thống **PHẢI** lưu trữ danh sách phòng học/phòng thi với: mã phòng, toà nhà, sức chứa tối đa, loại phòng (`lecture` / `lab` / `exam`). Hệ thống **PHẢI** kiểm tra và từ chối xếp lịch nếu phòng đã được sử dụng trong cùng tiết–thứ.

_UC liên quan:_ [`UC-004`](#uc-004), [`UC-011`](#uc-011), [`UC-012`](#uc-012).

---

#### FR-021 — Phân công giảng viên

<a id="fr-021"></a>

Hệ thống **PHẢI** cho phép Khoa/Phòng Đào tạo phân công giảng viên cho lớp học phần. Hệ thống **PHẢI** kiểm tra và từ chối nếu giảng viên bị trùng lịch. Hệ thống **NÊN** cảnh báo khi tổng số tiết giảng trong tuần của giảng viên vượt ngưỡng cấu hình (mặc định: 20 tiết/tuần).

_UC liên quan:_ [`UC-004`](#uc-004), [`UC-005`](#uc-005).

---

### 5.9. Nhóm FR: Quản lý đề thi

---

#### FR-022 — Quản lý thông tin đề thi

<a id="fr-022"></a>

Hệ thống **PHẢI** cho phép Phòng Khảo thí lưu thông tin đề thi: mã đề, học phần liên quan, học kỳ, trạng thái (`draft` / `approved` / `used`). Hệ thống **KHÔNG ĐƯỢC** lưu nội dung đề thi dưới dạng văn bản thuần trong CSDL — chỉ lưu tham chiếu file; bảo mật nội dung đề thuộc trách nhiệm quy trình vận hành ngoài phạm vi hệ thống.

_UC liên quan:_ [`UC-011`](#uc-011).

---

### 5.10. Nhóm FR: Xuất báo cáo & dữ liệu

---

#### FR-023 — Xuất báo cáo PDF / CSV

<a id="fr-023"></a>

Hệ thống **PHẢI** cho phép Lãnh đạo, Phòng Đào tạo và Khoa xuất báo cáo tổng hợp từ dashboard dưới dạng PDF. Hệ thống **NÊN** hỗ trợ thêm xuất CSV/Excel cho dữ liệu dạng bảng (danh sách sinh viên, bảng điểm, tổng hợp học phí sandbox).

_UC liên quan:_ [`UC-008`](#uc-008), [`UC-014`](#uc-014), [`UC-016`](#uc-016).

---

#### FR-024 — Nhật ký kiểm toán chi tiết

<a id="fr-024"></a>

Hệ thống **PHẢI** tự động ghi nhật ký kiểm toán (xem [`NFR-009`](#nfr-009)) cho: mọi thay đổi dữ liệu nghiệp vụ (điểm, đăng ký, phân công), mọi lần duyệt/từ chối AI output, mọi lần đăng nhập/đăng xuất/đổi mật khẩu. Mỗi bản ghi nhật ký **PHẢI** chứa: timestamp UTC, user ID, vai trò, hành động, ID đối tượng bị tác động, địa chỉ IP.

---

### 5.11. Nhóm FR: Thông báo

---

#### FR-017 — Thông báo tự động

<a id="fr-017"></a>

Hệ thống **PHẢI** gửi thông báo trong ứng dụng (_in-app notification_) cho các sự kiện: điểm công bố, đăng ký thành công/thất bại, kế hoạch học tập được duyệt/trả về, lịch thi công bố, cảnh báo học vụ.

---

#### FR-018 — Email và thông báo do AI soạn thảo

<a id="fr-018"></a>

Hệ thống **PHẢI** hỗ trợ AI soạn thảo email/thông báo hàng loạt (ví dụ: nhắc nộp học phí, thông báo lịch thi). Mọi nội dung AI soạn thảo gửi đến người dùng cuối **PHẢI** qua bước duyệt bởi nhân viên có thẩm quyền trước khi phát hành. Hệ thống **KHÔNG ĐƯỢC** gửi thông báo AI tự động mà không có người duyệt.

_UC liên quan:_ [`UC-008`](#uc-008).

---

## 6. Yêu cầu phi chức năng

### 6.1. Hiệu năng

---

#### NFR-001 — Thời gian phản hồi API nghiệp vụ

<a id="nfr-001"></a>

Hệ thống **PHẢI** đáp ứng 95% (_p95_) yêu cầu API nghiệp vụ (đăng ký học, xem điểm, xem thời khoá biểu đã lưu) trong vòng **300ms** khi có ≤ 200 người dùng đồng thời.

---

#### NFR-002 — Thời gian phản hồi AI gợi ý môn

<a id="nfr-002"></a>

Hệ thống **PHẢI** trả kết quả gợi ý môn học ([`UC-001`](#uc-001)) trong vòng **3s** ở _p95_ với ≤ 100 yêu cầu đồng thời.

---

#### NFR-003 — Thời gian sinh thời khoá biểu AI

<a id="nfr-003"></a>

Hệ thống **PHẢI** trả kết quả sinh thời khoá biểu cá nhân hoá ([`UC-002`](#uc-002)) trong vòng **8s** ở _p95_ với ≤ 50 yêu cầu đồng thời.

---

#### NFR-004 — Tải đồng thời tối đa

<a id="nfr-004"></a>

Hệ thống **PHẢI** duy trì hoạt động bình thường (không lỗi 5xx) khi có ≤ **500 người dùng đồng thời** trong đợt cao điểm đăng ký học.

---

### 6.2. Khả dụng

---

#### NFR-005 — Uptime

<a id="nfr-005"></a>

Hệ thống **PHẢI** đạt uptime ≥ **99%** trong mỗi tháng dương lịch (khoảng 7h downtime/tháng cho phép).

---

#### NFR-006 — Thời gian khôi phục

<a id="nfr-006"></a>

Sau sự cố nghiêm trọng, hệ thống **PHẢI** khôi phục trong vòng **2 giờ** (_RTO — Recovery Time Objective_). Dữ liệu tổn thất tối đa không quá **1 giờ** dữ liệu (_RPO — Recovery Point Objective_).

---

### 6.3. Bảo mật

---

#### NFR-007 — Phân quyền mặc định từ chối

<a id="nfr-007"></a>

Hệ thống **PHẢI** từ chối quyền truy cập theo mặc định; chỉ cấp quyền khi được khai báo tường minh cho vai trò tương ứng.

---

#### NFR-008 — Mã hoá dữ liệu

<a id="nfr-008"></a>

Hệ thống **PHẢI** mã hoá dữ liệu nhạy cảm (mật khẩu dùng bcrypt/argon2, dữ liệu PII) khi lưu trữ (_at-rest_). Toàn bộ giao tiếp client–server **PHẢI** qua TLS 1.2 trở lên (_in-transit_).

---

#### NFR-009 — Nhật ký kiểm toán

<a id="nfr-009"></a>

Hệ thống **PHẢI** ghi nhật ký kiểm toán cho: đăng nhập/đăng xuất, thay đổi dữ liệu học vụ (điểm, đăng ký, phân công), duyệt AI output. Nhật ký **KHÔNG ĐƯỢC** bị xoá hoặc sửa bởi bất kỳ vai trò nào ngoài SysAdmin (chỉ xem, không sửa).

---

### 6.4. Khả năng bảo trì

---

#### NFR-010 — Tách lớp

<a id="nfr-010"></a>

Kiến trúc **PHẢI** tách rõ lớp trình bày, ứng dụng, miền và hạ tầng. Phụ thuộc **PHẢI** một chiều từ ngoài vào trong (từ hạ tầng → domain, không ngược lại).

---

#### NFR-011 — Quan trắc

<a id="nfr-011"></a>

Hệ thống **PHẢI** xuất structured log (JSON) cho mọi yêu cầu API và mọi lời gọi AI. Hệ thống **PHẢI** có ít nhất 1 metric có thể vẽ đồ thị cho mỗi use case quan trọng.

---

#### NFR-012 — Lưu trữ nhật ký

<a id="nfr-012"></a>

Hệ thống **PHẢI** lưu trữ nhật ký kiểm toán ([`NFR-009`](#nfr-009)) tối thiểu **2 năm** trước khi được phép xoá. Nhật ký ứng dụng (application log) **NÊN** được giữ ≥ 90 ngày. Cả hai loại **PHẢI** được sao lưu ra ngoài cùng chu kỳ với dữ liệu nghiệp vụ.

---

#### NFR-013 — Độ phủ kiểm thử tối thiểu

<a id="nfr-013"></a>

Mã nguồn logic nghiệp vụ (lớp domain và lớp ứng dụng) **PHẢI** đạt độ phủ kiểm thử đơn vị (_unit test coverage_) ≥ **80%** theo số dòng. Mã nguồn verifier ([`BR-008`](#br-008)) **PHẢI** đạt ≥ **90%** độ phủ nhánh (_branch coverage_). Các ngưỡng này là điều kiện để đạt _Definition of Done_ của mỗi sprint (xem [`08-test-plan-acceptance-criteria.md`](08-test-plan-acceptance-criteria.md)).

---

### 6.5. Khả năng mở rộng

---

#### NFR-014 — Khả năng mở rộng theo chiều ngang

<a id="nfr-014"></a>

Kiến trúc backend **PHẢI** cho phép mở rộng theo chiều ngang (_horizontal scaling_) bằng cách thêm instance mà không cần thay đổi mã nguồn. Không có trạng thái phiên (_session state_) nào **ĐƯỢC** lưu trong bộ nhớ của từng instance — trạng thái **PHẢI** lưu trong CSDL hoặc cache dùng chung.

---

#### NFR-015 — Giới hạn tốc độ API

<a id="nfr-015"></a>

Hệ thống **PHẢI** áp dụng giới hạn tốc độ (_rate limiting_) cho các endpoint AI (gợi ý môn, sinh lịch, hội thoại) để bảo vệ ngân sách token LLM. Ngưỡng mặc định: ≤ **30 yêu cầu/phút/tài khoản** cho endpoint AI; ≤ **300 yêu cầu/phút/tài khoản** cho endpoint nghiệp vụ thông thường. Ngưỡng **NÊN** có thể cấu hình qua biến môi trường mà không cần deploy lại.

---

> Mỗi năng lực AI **PHẢI** có ít nhất 1 chỉ số đánh giá định lượng kèm: phương pháp đo, bộ dữ liệu đánh giá, ngưỡng đậu. Xem [`doc/context/01-muc-tieu-nghien-cuu-ai.md §5.3`](../context/01-muc-tieu-nghien-cuu-ai.md#53-chỉ-số-đo) để biết định nghĩa chi tiết M1–M6.

---

#### AI-001 — Gợi ý môn học theo chương trình

<a id="ai-001"></a>

Hệ thống **PHẢI** cung cấp năng lực gợi ý môn học cá nhân hoá dựa trên: tiến độ chương trình, điều kiện tiên quyết, lịch sử học tập của sinh viên.

| Thuộc tính          | Giá trị                                                                     |
| :------------------ | :-------------------------------------------------------------------------- |
| Chỉ số đánh giá     | CCR — _Constraint Capture Rate_: tỉ lệ ràng buộc (tiên quyết, tiến độ) được hiểu đúng. |
| Phương pháp đo      | Chạy trên tập VACS (bộ con `easy`) với 100 examples; so sánh với gold label. |
| Ngưỡng đậu          | CCR ≥ 0.85                                                                  |
| Fallback khi AI lỗi | Hệ thống **PHẢI** fallback sang gợi ý dựa quy tắc đơn giản (lọc tiên quyết).     |

_UC liên quan:_ [`UC-001`](#uc-001). _FR liên quan:_ không có FR riêng (AI là cơ chế của FR-004, FR-007).

---

#### AI-002 — Sinh thời khoá biểu cá nhân hoá (Hướng nghiên cứu chính)

<a id="ai-002"></a>

Hệ thống **PHẢI** cung cấp năng lực sinh thời khoá biểu không trùng lịch từ yêu cầu ngôn ngữ tự nhiên của sinh viên, sử dụng kiến trúc Pure LLM kèm verifier (hướng γ theo [`doc/context/01-muc-tieu-nghien-cuu-ai.md §4`](../context/01-muc-tieu-nghien-cuu-ai.md#4-quyết-định-chốt-hướng-nghiên-cứu-c--pure-llm-end-to-end)).

| Thuộc tính            | Giá trị                                                                                      |
| :-------------------- | :------------------------------------------------------------------------------------------- |
| Chỉ số M1 — SVR       | _Schedule Validity Rate_: tỉ lệ lịch sinh ra không vi phạm ràng buộc cứng (verifier check). |
| Ngưỡng đậu M1         | SVR ≥ 0.90 × SVR(α) trên tập VACS full (300 examples).                                      |
| Chỉ số M2 — CCR       | _Constraint Capture Rate_: ràng buộc người dùng phát biểu được hiểu đúng. Ngưỡng: ≥ 0.85.  |
| Chỉ số M3 — UPS       | _User Preference Score_: đánh giá chủ quan 1-5 từ ≥ 30 sinh viên trong human study. Ngưỡng: trung bình ≥ 3.8.  |
| Chỉ số M4 — Latency   | Thời gian phản hồi p95 ≤ 8s (xem [`NFR-003`](#nfr-003)).                                    |
| Chỉ số M5 — Token     | Trung bình ≤ 8000 token (in + out) / lượt.                                                   |
| Chỉ số M6 — Soft      | Điểm tiêu chí mềm ≥ 0.75 trên rubric của VACS.                                              |
| Phương pháp đo M1/M2  | Chạy cấu hình γ₀–γ₄ trên tập VACS; mỗi cấu hình ≥ 3 lần độc lập (bootstrap CI).            |
| Fallback khi AI lỗi   | Hệ thống **PHẢI** fallback sang bộ lọc thủ công (α — pure solver) khi LLM không trả lời trong ngưỡng thời gian. |

_UC liên quan:_ [`UC-002`](#uc-002).

---

#### AI-003 — Cảnh báo bất thường đào tạo

<a id="ai-003"></a>

Hệ thống **PHẢI** phát hiện và cảnh báo bất thường: lớp quá tải, giảng viên chưa phân công, xu hướng tỉ lệ rớt môn cao.

| Thuộc tính      | Giá trị                                                                                       |
| :-------------- | :-------------------------------------------------------------------------------------------- |
| Chỉ số đánh giá | _Precision_ cảnh báo: tỉ lệ cảnh báo đúng (không phải nhiễu) trên tập 50 tình huống kiểm thử. |
| Ngưỡng đậu      | Precision ≥ 0.80                                                                              |
| Phương pháp đo  | Nhóm phát triển tạo 50 tình huống có nhãn (30 dương tính, 20 âm tính); đo precision.         |

_UC liên quan:_ [`UC-005`](#uc-005).

---

#### AI-004 — Hội thoại học vụ có trích dẫn nguồn

<a id="ai-004"></a>

Hệ thống **PHẢI** cung cấp chatbot học vụ trả lời dựa trên corpus tài liệu nội bộ, **PHẢI** trích dẫn nguồn cụ thể cho mỗi phát biểu, **KHÔNG ĐƯỢC** trả lời chỉ bằng kiến thức nền của mô hình khi câu hỏi liên quan đến quy chế/quy định cụ thể.

| Thuộc tính      | Giá trị                                                                                    |
| :-------------- | :----------------------------------------------------------------------------------------- |
| Chỉ số đánh giá | _Citation Accuracy_: tỉ lệ câu trả lời có ít nhất 1 trích dẫn nguồn đúng và có thể kiểm chứng. |
| Ngưỡng đậu      | Citation Accuracy ≥ 0.90 trên tập 100 câu hỏi kiểm thử.                                   |
| Phương pháp đo  | Nhóm phát triển soạn 100 câu hỏi học vụ có đáp án gold; đánh giá thủ công câu trích dẫn. |

_UC liên quan:_ [`UC-015`](#uc-015).

---

#### AI-005 — Dự báo nguy cơ học vụ

<a id="ai-005"></a>

Hệ thống **NÊN** dự báo sớm sinh viên có nguy cơ cảnh báo hoặc buộc thôi học dựa trên: xu hướng điểm, số môn rớt, tỉ lệ đăng ký so với chương trình.

| Thuộc tính      | Giá trị                                                                                     |
| :-------------- | :------------------------------------------------------------------------------------------ |
| Chỉ số đánh giá | _Recall_ trên tập sinh viên thực tế bị cảnh báo học vụ cuối kỳ (backtest trên dữ liệu lịch sử nếu có). |
| Ngưỡng đậu      | Recall ≥ 0.70 với Precision ≥ 0.60 (F1 ≥ 0.64)                                             |
| Phương pháp đo  | Backtest trên ≥ 2 học kỳ dữ liệu lịch sử (ẩn danh hoá); hoặc giả lập nếu không có dữ liệu thật. |

_UC liên quan:_ [`UC-010`](#uc-010).

---

#### AI-006 — Sinh lịch thi tối ưu

<a id="ai-006"></a>

Hệ thống **PHẢI** hỗ trợ Phòng Khảo thí sinh lịch thi giảm xung đột sinh viên và cân bằng tải phòng.

| Thuộc tính      | Giá trị                                                                                     |
| :-------------- | :------------------------------------------------------------------------------------------- |
| Chỉ số đánh giá | _Conflict Rate_: tỉ lệ sinh viên bị trùng ≥ 2 lịch thi trong cùng 1 ngày trên đề kiểm thử. |
| Ngưỡng đậu      | Conflict Rate ≤ 5% (so với baseline ngẫu nhiên thường ≥ 20-30%).                            |
| Phương pháp đo  | Tạo 10 bộ dữ liệu giả lập (lớp, SV, phòng); so sánh AI vs phân công ngẫu nhiên.            |

_UC liên quan:_ [`UC-011`](#uc-011).

---

#### AI-007 — Phân tích cảm xúc và tóm tắt feedback giảng dạy

<a id="ai-007"></a>

Hệ thống **PHẢI** phân tích cảm xúc (_sentiment analysis_) và phân loại chủ đề (_topic classification_) trên nhận xét tự do của sinh viên, sinh tóm tắt per giảng viên / per môn.

| Thuộc tính      | Giá trị                                                                                       |
| :-------------- | :-------------------------------------------------------------------------------------------- |
| Chỉ số đánh giá | _Sentiment Accuracy_: khớp với nhãn con người trên tập 200 nhận xét kiểm thử.                |
| Ngưỡng đậu      | Sentiment Accuracy ≥ 0.80 (phân loại 3 nhãn: tích cực / trung lập / tiêu cực).               |
| Phương pháp đo  | Nhóm phát triển gán nhãn 200 nhận xét thật (hoặc giả lập); đo accuracy.                     |

_UC liên quan:_ [`UC-014`](#uc-014).

---

#### AI-008 — Soạn thảo email / thông báo hàng loạt

<a id="ai-008"></a>

Hệ thống **PHẢI** hỗ trợ AI soạn nháp email và thông báo từ template và dữ liệu học vụ. Mọi nội dung **PHẢI** qua bước duyệt của người có thẩm quyền trước khi gửi ([`FR-018`](#fr-018)).

| Thuộc tính      | Giá trị                                                                                 |
| :-------------- | :-------------------------------------------------------------------------------------- |
| Chỉ số đánh giá | Tỉ lệ nháp được người duyệt chấp nhận không cần sửa đáng kể (≤ 20% chỉnh sửa từ nháp). |
| Ngưỡng đậu      | ≥ 0.70 nháp được chấp nhận trực tiếp trên 50 nháp kiểm thử.                            |
| Phương pháp đo  | Thu thập phản hồi của người duyệt trong giai đoạn thử nghiệm nội bộ.                   |

_UC liên quan:_ [`UC-008`](#uc-008).

---

## 8. Ràng buộc & giả định

### 8.1. Ràng buộc cứng (không thương lượng)

| Mã       | Ràng buộc                                                                                                              |
| :------- | :--------------------------------------------------------------------------------------------------------------------- |
| `BR-001` | Thanh toán học phí **chỉ hoạt động ở môi trường sandbox**. Nghiêm cấm tích hợp cổng thanh toán thật, lưu thông tin tài khoản ngân hàng hay thẻ thật. |
| `BR-002` | Mọi nội dung AI gửi ra người dùng cuối (email, thông báo) **PHẢI qua bước người duyệt**. Không có AI tự động gửi khi chưa được duyệt. |
| `BR-003` | Hội thoại học vụ AI **PHẢI trích dẫn nguồn**. AI không được trả lời về quy chế/quy định chỉ bằng kiến thức nền của mô hình. |
| `BR-004` | Kết quả khảo sát đánh giá giảng dạy **PHẢI ẩn danh**. Nghiêm cấm lưu hoặc suy luận danh tính sinh viên từ câu trả lời khảo sát. |
| `BR-005` | Miễn/giảm học phí theo chính sách **nằm ngoài phạm vi MVP**. Không triển khai tính năng này. |
| `BR-006` | Nền tảng **chỉ áp dụng trong phạm vi một khoa/trường**. Không thiết kế đa thuê bao (_multi-tenant_) phân cấp trường. |
| `BR-007` | Lãnh đạo cấp trường **chỉ đọc** (_read-only_). Không tạo giao diện nhập/sửa dữ liệu cho vai trò này. |
| `BR-008` | Verifier trong luồng AI sinh lịch **PHẢI là hàm thuần kiểm tra**, không phải _constraint solver_. Nghiêm cấm dùng OR-Tools/MiniZinc trong luồng suy luận chính của γ. |

### 8.2. Giả định

| Mã       | Giả định                                                                                                                 |
| :------- | :----------------------------------------------------------------------------------------------------------------------- |
| `ASS-001` | Trường/khoa có thể cung cấp quy chế đào tạo, chương trình đào tạo dạng văn bản có thể đọc được để nạp vào corpus RAG. |
| `ASS-002` | API LLM từ ít nhất 2 nhà cung cấp (Anthropic, OpenAI, hoặc tương đương) sẵn có và ổn định trong vòng 6 tháng từ khi bắt đầu thí nghiệm. |
| `ASS-003` | Có thể tiếp cận ≥ 30 sinh viên cho human study ([`AI-002`](#ai-002) — M3) theo quy trình được cố vấn/nhà trường cho phép. |
| `ASS-004` | Hạ tầng triển khai là cloud hoặc server vật lý do nhóm phát triển quản lý; không phụ thuộc hệ thống CNTT hiện tại của trường. |

---

## 9. Glossary

> Các thuật ngữ học vụ tiếng Việt dùng trong toàn bộ tài liệu NCKH_1. Thuật ngữ kỹ thuật giữ nguyên tiếng Anh, kèm chú thích lần đầu.

**Học phần** (_Course_) — đơn vị kiến thức được giảng dạy, có mã, tên, số tín chỉ, mô tả, học phần tiên quyết. Một học phần có thể được mở thành nhiều _lớp học phần_ trong mỗi học kỳ.

**Lớp học phần** (_Course offering_) — một lần mở học phần X trong học kỳ Y với giảng viên, phòng học, thời gian xác định. Sinh viên đăng ký vào _lớp học phần_, không phải _học phần_.

**Học kỳ** (_Semester_) — đơn vị thời gian đào tạo; thông thường 1 năm học có 2–3 học kỳ (bao gồm học kỳ hè).

**Tín chỉ** (_Credit_) — đơn vị đo khối lượng học tập; 1 tín chỉ tương đương ~15 tiết lý thuyết hoặc ~30 tiết thực hành.

**Chương trình đào tạo** (_Curriculum_) — danh sách học phần bắt buộc và tự chọn mà sinh viên phải hoàn thành để tốt nghiệp một ngành cụ thể.

**Tiên quyết** (_Prerequisite_) — học phần A là tiên quyết của B nếu sinh viên phải hoàn thành (đạt) A trước khi đăng ký B.

**GPA** — _Grade Point Average_ (điểm trung bình tích luỹ); tính theo thang điểm 4.

**Tình trạng học vụ** — phân loại: bình thường, cảnh báo lần 1, cảnh báo lần 2, buộc thôi học; căn cứ theo quy chế đào tạo của trường.

**CVHT** — Cố vấn học tập (_Academic advisor_); vai trò bổ sung gắn trên tài khoản giảng viên.

**Verifier** — hàm kiểm tra thuần (_pure function_) nhận vào 1 lịch đề xuất và trả về danh sách ràng buộc bị vi phạm (nếu có). Không tìm lời giải, chỉ kiểm tra. Phân biệt với _solver_.

**Solver** (_Constraint solver_) — thuật toán tìm kiếm tạo ra lời giải thoả ràng buộc (OR-Tools, MiniZinc, ...). Bị cấm trong luồng suy luận chính của γ ([`BR-008`](#br-008)).

**VACS** — _Vietnamese Academic Constraint Set_; bộ dữ liệu benchmark cho việc đánh giá năng lực sinh lịch AI, gồm ≥ 300 examples với gold solution.

**RAG** — _Retrieval-Augmented Generation_; kỹ thuật AI kết hợp truy vấn tài liệu nội bộ với sinh văn bản.

**CoT** — _Chain-of-Thought_; kỹ thuật nhắc mô hình suy luận từng bước.

**Tiết học** (_Slot_) — đơn vị thời gian nhỏ nhất trong thời khoá biểu; thường kéo dài 45–50 phút. Một buổi học có thể gồm 2–3 tiết liên tiếp.

**Thang điểm 10 / Thang điểm 4** — hai thang điểm phổ biến ở Việt Nam. Thang 10: điểm từ 0–10 (thường dùng khi nhập điểm từng thành phần). Thang 4: điểm từ 0.0–4.0 dùng để tính GPA theo tín chỉ. Quy đổi theo bảng quy chế của từng trường.

**Học phần tương đương** (_Equivalent course_) — học phần B được công nhận thay thế cho học phần A trong chương trình đào tạo (sinh viên chuyển ngành, chuyển trường).

**Trạng thái kế hoạch học tập** — vòng đời kế hoạch: `draft` (nháp, chưa gửi) → `submitted` (đã gửi CVHT) → `approved` (được duyệt) → `rejected` (trả lại). Kế hoạch `approved` mới được dùng để thực hiện đăng ký chính thức.

**Idempotency-Key** — chuỗi định danh duy nhất do client tạo ra và gửi kèm yêu cầu; đảm bảo rằng nếu cùng một yêu cầu được gửi nhiều lần (do mạng bất ổn), hệ thống chỉ xử lý một lần duy nhất.

**Sandbox** — môi trường giả lập giao dịch tài chính; không có tiền thật, không kết nối cổng thanh toán thật. Mọi giao dịch đều là dữ liệu mô phỏng.

**PII** — _Personally Identifiable Information_ (thông tin nhận dạng cá nhân); bao gồm: họ tên, mã số sinh viên, ngày sinh, địa chỉ, số điện thoại, email. Cần bảo vệ theo quy định bảo vệ dữ liệu cá nhân.

---

## 10. Ma trận truy ngược (sơ bộ)

> Ma trận này sẽ được mở rộng và duy trì sau khi `02-hld.md` hoàn thành. Mỗi commit thêm UC/FR **PHẢI** cập nhật bảng này.

| Use Case                               | Yêu cầu chức năng liên quan        | Yêu cầu phi chức năng liên quan | Yêu cầu AI liên quan                         |
| :------------------------------------- | :--------------------------------- | :------------------------------ | :------------------------------------------- |
| [UC-001](#uc-001) Gợi ý môn học        | FR-004, FR-007                     | NFR-002, NFR-004                | [AI-001](#ai-001)                            |
| [UC-002](#uc-002) Sinh thời khoá biểu  | FR-006, FR-007, FR-019, FR-020     | NFR-003, NFR-004, NFR-015       | [AI-002](#ai-002)                            |
| [UC-003](#uc-003) Duyệt kế hoạch       | FR-002, FR-017                     | NFR-001                         | —                                            |
| [UC-004](#uc-004) Quản lý lớp          | FR-006, FR-019, FR-020, FR-021     | NFR-001, NFR-009, NFR-024       | —                                            |
| [UC-005](#uc-005) Cảnh báo bất thường  | FR-017, FR-021                     | NFR-001, NFR-011                | [AI-003](#ai-003)                            |
| [UC-006](#uc-006) Đăng ký học phần     | FR-004, FR-007, FR-008, FR-019     | NFR-001, NFR-004, NFR-007       | —                                            |
| [UC-007](#uc-007) Xem học phí          | FR-009, FR-010                     | NFR-001                         | —                                            |
| [UC-008](#uc-008) Theo dõi học phí     | FR-009, FR-018, FR-023             | NFR-001                         | [AI-008](#ai-008)                            |
| [UC-009](#uc-009) Nhập điểm            | FR-011, FR-012, FR-017             | NFR-001, NFR-009, NFR-024       | —                                            |
| [UC-010](#uc-010) Bảng điểm            | FR-013                             | NFR-001                         | [AI-005](#ai-005)                            |
| [UC-011](#uc-011) Lập lịch thi         | FR-014, FR-015, FR-019, FR-020, FR-022 | NFR-001                     | [AI-006](#ai-006)                            |
| [UC-012](#uc-012) Phân phòng/giám thị  | FR-015, FR-020                     | NFR-001                         | —                                            |
| [UC-013](#uc-013) Khảo sát             | FR-016                             | NFR-007, NFR-008                | —                                            |
| [UC-014](#uc-014) Xem đánh giá         | FR-016, FR-023                     | NFR-001, NFR-007                | [AI-007](#ai-007)                            |
| [UC-015](#uc-015) Hỏi đáp học vụ      | FR-017                             | NFR-002, NFR-015                | [AI-004](#ai-004)                            |
| [UC-016](#uc-016) Dashboard lãnh đạo   | FR-002, FR-013, FR-023             | NFR-001, NFR-007                | [AI-003](#ai-003), [AI-007](#ai-007)         |

---

## 11. Tham chiếu

- [`../../CLAUDE.md`](../../CLAUDE.md) — phạm vi & ranh giới dự án; nguồn sự thật cho §1.5 ràng buộc.
- [`00-quy-chuan.md`](00-quy-chuan.md) — quy chuẩn tài liệu (đặc biệt §5 ID, §6 RFC 2119, §12.3 đo lường).
- [`../agents.md`](../agents.md) — chỉ dẫn cho agent; §3.1 nội dung tối thiểu SRS.
- [`../context/01-muc-tieu-nghien-cuu-ai.md`](../context/01-muc-tieu-nghien-cuu-ai.md) — hướng nghiên cứu AI, khung thí nghiệm α/β/γ, chỉ số M1–M6.
- [`../context/02-ke-hoach-chuan-bi-nghien-cuu.md`](../context/02-ke-hoach-chuan-bi-nghien-cuu.md) — kế hoạch chuẩn bị; schema VACS; phạm vi verifier.
- ISO/IEC/IEEE 29148:2018 — Systems and software engineering — Requirements engineering.
- RFC 2119 / RFC 8174 — Key words for use in RFCs to Indicate Requirement Levels.
