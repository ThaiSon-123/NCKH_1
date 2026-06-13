# 05 — Đặc tả API (API Specification)

## 1. Thông tin tài liệu

### 1.1. Metadata

| Thuộc tính | Giá trị |
| :-- | :-- |
| Tên tài liệu | Đặc tả API (_API Specification_) |
| Mã tài liệu | 05-api-specification |
| Dự án | Nền tảng tích hợp hỗ trợ tổ chức đào tạo (NCKH_1) |
| Phiên bản | v0.1.0 |
| Trạng thái | Draft |
| Người viết | AI Agent |
| Người duyệt | (chưa duyệt) |
| Ngày tạo | 2026-06-13 |
| Ngày cập nhật | 2026-06-13 |
| Tài liệu liên quan | [`../../CLAUDE.md`](../../CLAUDE.md), [`00-quy-chuan.md`](00-quy-chuan.md), [`03-lld.md`](03-lld.md), [`04-database-design.md`](04-database-design.md) |

### 1.2. Lịch sử thay đổi (Changelog)

| Phiên bản | Ngày | Tác giả | Thay đổi |
| :-- | :-- | :-- | :-- |
| 0.1.0 | 2026-06-13 | AI Agent | Tạo skeleton API Specification. |

---

## 2. Mục đích & phạm vi

Tài liệu này sẽ mô tả API theo OpenAPI 3.1, nhóm endpoint theo resource, schema, error model, phân quyền, rate limit và idempotency.

## 3. Quy ước API

| Chủ đề | Quy ước dự kiến | Trạng thái |
| :-- | :-- | :-- |
| Base path | `/api/v1` | TBD |
| Format | JSON UTF-8 | TBD |
| Auth | TBD | TBD |
| Error model | `code`, `message`, `details`, `traceId` | TBD |

## 4. OpenAPI 3.1 Skeleton

```yaml
openapi: 3.1.0
info:
  title: NCKH_1 API
  version: 0.1.0
paths: {}
components:
  schemas: {}
  securitySchemes: {}
```

## 5. Nhóm endpoint theo resource

| Resource | Endpoint dự kiến | FR/UC |
| :-- | :-- | :-- |
| Auth/User | `/auth`, `/users` | FR-001, FR-003, UC-018 |
| Course/Curriculum | `/curricula`, `/courses` | FR-004, FR-005, UC-017 |
| Registration | `/registrations`, `/waitlists` | FR-007, FR-008, UC-006 |
| AI | `/ai/schedule`, `/ai/qa`, `/ai/drafts` | AI-002, AI-004, AI-008 |

## 6. Request/response schema

TBD: schema, ví dụ payload, mã lỗi và role được gọi cho từng endpoint.

## 7. Xác thực & phân quyền API

TBD: RBAC/ABAC, default deny, condition theo actor và resource ownership.

## 8. Rate limit & idempotency

| Loại endpoint | Quy tắc | Liên quan |
| :-- | :-- | :-- |
| AI | TBD theo `NFR-015` | AI-001 … AI-008 |
| Đăng ký học phần | Bắt buộc `Idempotency-Key` | FR-007 |
| Thanh toán sandbox | Bắt buộc `Idempotency-Key` | FR-010 |

## 9. Mapping endpoint ↔ FR/UC

| API ID | Method/path | FR/UC | Trạng thái |
| :-- | :-- | :-- | :-- |
| API-001 | TBD | TBD | TBD |

## 10. Open Questions

| Mã | Câu hỏi mở | Tác động |
| :-- | :-- | :-- |
| API-OPEN-001 | Cookie session hay JWT bearer? | Ảnh hưởng bảo mật và logout. |
| API-OPEN-002 | Có tách API nội bộ cho AI worker không? | Ảnh hưởng HLD/LLD. |

## 11. Tham chiếu

- [`03-lld.md`](03-lld.md)
- [`04-database-design.md`](04-database-design.md)
- [`07-security-permission-design.md`](07-security-permission-design.md)
- OpenAPI 3.1 Specification