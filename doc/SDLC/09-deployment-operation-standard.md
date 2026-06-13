# 09 — Chuẩn Triển khai & Vận hành (Deployment & Operation Standard)

## 1. Thông tin tài liệu

### 1.1. Metadata

| Thuộc tính | Giá trị |
| :-- | :-- |
| Tên tài liệu | Chuẩn Triển khai & Vận hành (_Deployment & Operation Standard_) |
| Mã tài liệu | 09-deployment-operation-standard |
| Dự án | Nền tảng tích hợp hỗ trợ tổ chức đào tạo (NCKH_1) |
| Phiên bản | v0.1.0 |
| Trạng thái | Draft |
| Người viết | AI Agent |
| Người duyệt | (chưa duyệt) |
| Ngày tạo | 2026-06-13 |
| Ngày cập nhật | 2026-06-13 |
| Tài liệu liên quan | [`../../CLAUDE.md`](../../CLAUDE.md), [`00-quy-chuan.md`](00-quy-chuan.md), [`02-hld.md`](02-hld.md), [`07-security-permission-design.md`](07-security-permission-design.md) |

### 1.2. Lịch sử thay đổi (Changelog)

| Phiên bản | Ngày | Tác giả | Thay đổi |
| :-- | :-- | :-- | :-- |
| 0.1.0 | 2026-06-13 | AI Agent | Tạo skeleton Deployment & Operation Standard. |

---

## 2. Mục đích & phạm vi

Tài liệu này sẽ mô tả môi trường, CI/CD, cấu hình, quan trắc, backup/restore và runbook vận hành.

## 3. Môi trường

| Môi trường | Mục đích | Dữ liệu được phép | Trạng thái |
| :-- | :-- | :-- | :-- |
| Local | Phát triển cá nhân | Dữ liệu giả lập | TBD |
| Dev | Tích hợp nội bộ | Dữ liệu giả lập/ẩn danh | TBD |
| Staging | Kiểm thử trước nghiệm thu | Dữ liệu giả lập/ẩn danh | TBD |
| Production | Vận hành thật nếu có | Theo phê duyệt | TBD |

## 4. Cấu hình & secrets

TBD: biến môi trường, secret manager, API key LLM, budget alert, sandbox config.

## 5. CI/CD pipeline

| Stage | Kiểm tra | Điều kiện promote |
| :-- | :-- | :-- |
| Lint | TBD | Không lỗi |
| Test | TBD | Test xanh |
| Build | TBD | Artifact thành công |
| Deploy | TBD | Phê duyệt theo môi trường |

## 6. Quy trình triển khai

TBD: deploy, rollback, migration database, seed data, kiểm tra sau deploy.

## 7. Observability

| Loại tín hiệu | Nội dung | Người nhận |
| :-- | :-- | :-- |
| Log | Structured JSON log | TBD |
| Metric | Latency, error rate, token cost, queue lag | TBD |
| Trace | Request trace luồng quan trọng | TBD |
| Alert | Budget LLM, 5xx, job fail, backup fail | TBD |

## 8. Sao lưu & khôi phục

TBD: RTO/RPO theo `NFR-006`, lịch backup, restore test, mã hoá backup.

## 9. Runbook vận hành

| Sự cố | Triệu chứng | Cách xử lý | Escalation |
| :-- | :-- | :-- | :-- |
| API 5xx tăng | TBD | TBD | TBD |
| LLM lỗi/quota hết | TBD | TBD | TBD |
| Job báo cáo thất bại | TBD | TBD | TBD |
| Backup fail | TBD | TBD | TBD |

## 10. Security operations

TBD: rotate secrets, kiểm tra audit log, xử lý tài khoản bị khoá, review quyền định kỳ.

## 11. Open Questions

| Mã | Câu hỏi mở | Tác động |
| :-- | :-- | :-- |
| OPS-OPEN-001 | Deploy trên cloud hay server vật lý? | Ảnh hưởng vận hành và backup. |
| OPS-OPEN-002 | Có production thật trong phạm vi luận văn không? | Ảnh hưởng scope bảo mật. |

## 12. Tham chiếu

- [`02-hld.md`](02-hld.md)
- [`07-security-permission-design.md`](07-security-permission-design.md)
- [`08-test-plan-acceptance-criteria.md`](08-test-plan-acceptance-criteria.md)
- [`00-quy-chuan.md`](00-quy-chuan.md)