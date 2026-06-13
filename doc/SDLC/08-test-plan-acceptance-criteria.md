# 08 — Kế hoạch Kiểm thử & Tiêu chí Nghiệm thu (Test Plan & Acceptance Criteria)

## 1. Thông tin tài liệu

### 1.1. Metadata

| Thuộc tính | Giá trị |
| :-- | :-- |
| Tên tài liệu | Kế hoạch Kiểm thử & Tiêu chí Nghiệm thu (_Test Plan & Acceptance Criteria_) |
| Mã tài liệu | 08-test-plan-acceptance-criteria |
| Dự án | Nền tảng tích hợp hỗ trợ tổ chức đào tạo (NCKH_1) |
| Phiên bản | v0.1.0 |
| Trạng thái | Draft |
| Người viết | AI Agent |
| Người duyệt | (chưa duyệt) |
| Ngày tạo | 2026-06-13 |
| Ngày cập nhật | 2026-06-13 |
| Tài liệu liên quan | [`../../CLAUDE.md`](../../CLAUDE.md), [`00-quy-chuan.md`](00-quy-chuan.md), [`01-srs.md`](01-srs.md), [`05-api-specification.md`](05-api-specification.md), [`06-ui-ux-flow-specification.md`](06-ui-ux-flow-specification.md) |

### 1.2. Lịch sử thay đổi (Changelog)

| Phiên bản | Ngày | Tác giả | Thay đổi |
| :-- | :-- | :-- | :-- |
| 0.1.0 | 2026-06-13 | AI Agent | Tạo skeleton Test Plan & Acceptance Criteria. |

---

## 2. Mục đích & phạm vi

Tài liệu này sẽ mô tả chiến lược test, test case, acceptance criteria, kiểm thử AI và Definition of Done.

## 3. Chiến lược kiểm thử

| Loại test | Phạm vi | Công cụ | Trạng thái |
| :-- | :-- | :-- | :-- |
| Unit | Logic nghiệp vụ, verifier | TBD | TBD |
| Integration | API, DB, AI wrapper | TBD | TBD |
| E2E | Luồng UI chính | TBD | TBD |
| AI evaluation | VACS, gold set, feedback labels | TBD | TBD |

## 4. Acceptance criteria theo epic

| Epic | GWT cần viết | UC/FR | Trạng thái |
| :-- | :-- | :-- | :-- |
| Kế hoạch & đăng ký học tập | TBD | UC-001 … UC-006 | TBD |
| Tài chính sandbox | TBD | UC-007, UC-008 | TBD |
| Kết quả học tập | TBD | UC-009, UC-010 | TBD |
| Khảo thí | TBD | UC-011, UC-012 | TBD |
| AI xuyên suốt | TBD | AI-001 … AI-008 | TBD |

## 5. Bộ ca kiểm thử

| TC | Mục tiêu | Given | When | Then | Mapping |
| :-- | :-- | :-- | :-- | :-- | :-- |
| TC-001 | TBD | TBD | TBD | TBD | TBD |

## 6. Đánh giá AI

| AI | Dataset/gold set | Metric | Ngưỡng từ SRS |
| :-- | :-- | :-- | :-- |
| AI-001 | VACS subset | CCR | ≥ 0.85 |
| AI-002 | VACS-300 | M1–M6 | Theo SRS |
| AI-004 | 100 câu hỏi gold | Citation Accuracy | ≥ 0.90 |
| AI-007 | 200 nhận xét gán nhãn | Sentiment Accuracy | ≥ 0.80 |

## 7. Test data & môi trường

TBD: seed dữ liệu, dữ liệu giả lập, dữ liệu ẩn danh, CI, staging và quản lý API key test AI.

## 8. Definition of Done

| Hạng mục | Tiêu chí |
| :-- | :-- |
| Code | TBD |
| Tài liệu | TBD |
| AI | Metric đạt ngưỡng hoặc ghi rõ kết quả |
| Bảo mật | Test phân quyền và audit log đạt |

## 9. Traceability TC ↔ UC/FR/AI

| TC | UC | FR/NFR/AI | Ghi chú |
| :-- | :-- | :-- | :-- |
| TBD | TBD | TBD | TBD |

## 10. Open Questions

| Mã | Câu hỏi mở | Tác động |
| :-- | :-- | :-- |
| TEST-OPEN-001 | Dùng công cụ E2E nào? | Ảnh hưởng CI và UI test. |
| TEST-OPEN-002 | VACS/gold set lưu ở đâu? | Ảnh hưởng reproducibility. |

## 11. Tham chiếu

- [`01-srs.md`](01-srs.md)
- [`05-api-specification.md`](05-api-specification.md)
- [`06-ui-ux-flow-specification.md`](06-ui-ux-flow-specification.md)
- [`../context/01-muc-tieu-nghien-cuu-ai.md`](../context/01-muc-tieu-nghien-cuu-ai.md)