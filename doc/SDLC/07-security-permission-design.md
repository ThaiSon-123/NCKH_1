# 07 — Thiết kế Bảo mật & Phân quyền (Security & Permission Design)

## 1. Thông tin tài liệu

### 1.1. Metadata

| Thuộc tính | Giá trị |
| :-- | :-- |
| Tên tài liệu | Thiết kế Bảo mật & Phân quyền (_Security & Permission Design_) |
| Mã tài liệu | 07-security-permission-design |
| Dự án | Nền tảng tích hợp hỗ trợ tổ chức đào tạo (NCKH_1) |
| Phiên bản | v0.1.0 |
| Trạng thái | Draft |
| Người viết | AI Agent |
| Người duyệt | (chưa duyệt) |
| Ngày tạo | 2026-06-13 |
| Ngày cập nhật | 2026-06-13 |
| Tài liệu liên quan | [`../../CLAUDE.md`](../../CLAUDE.md), [`00-quy-chuan.md`](00-quy-chuan.md), [`01-srs.md`](01-srs.md), [`04-database-design.md`](04-database-design.md), [`05-api-specification.md`](05-api-specification.md) |

### 1.2. Lịch sử thay đổi (Changelog)

| Phiên bản | Ngày | Tác giả | Thay đổi |
| :-- | :-- | :-- | :-- |
| 0.1.0 | 2026-06-13 | AI Agent | Tạo skeleton Security & Permission Design. |

---

## 2. Mục đích & phạm vi

Tài liệu này sẽ mô tả xác thực, RBAC/ABAC, bảo vệ dữ liệu, STRIDE, audit log và tiêu chí bảo mật.

## 3. Mô hình phân quyền

TBD: RBAC theo 8 vai trò trong SRS, ABAC theo phạm vi dữ liệu.

## 4. Ma trận quyền

| Vai trò | Tài nguyên | Hành động | Điều kiện |
| :-- | :-- | :-- | :-- |
| TBD | TBD | TBD | TBD |

## 5. Xác thực & quản lý phiên

TBD: email/mật khẩu, SSO nếu có, session/JWT, logout, CSRF, reset mật khẩu.

## 6. Bảo vệ dữ liệu

| Loại dữ liệu | Biện pháp | Liên quan |
| :-- | :-- | :-- |
| Mật khẩu | Hash argon2/bcrypt | NFR-008 |
| PII | TBD | NFR-008 |
| Khảo sát ẩn danh | TBD | BR-004 |
| Audit log | Không sửa được | FR-024, NFR-009 |

## 7. Threat model (STRIDE)

| Luồng nhạy cảm | Rủi ro chính | Biện pháp |
| :-- | :-- | :-- |
| Đăng ký học phần | TBD | TBD |
| Thanh toán sandbox | TBD | TBD |
| AI soạn thông báo | TBD | TBD |
| Khảo sát ẩn danh | TBD | TBD |

## 8. Nhật ký kiểm toán (Audit log)

TBD: sự kiện, cấu trúc log, thời hạn lưu, quyền xem, chống sửa.

## 9. Security acceptance criteria

| Mã | Tiêu chí | Cách kiểm chứng |
| :-- | :-- | :-- |
| SEC-AC-001 | Default deny | Test phân quyền API/UI |
| SEC-AC-002 | SysAdmin không đọc/sửa dữ liệu nghiệp vụ | Test negative |
| SEC-AC-003 | AI output gửi ra ngoài phải qua duyệt | Test luồng duyệt |

## 10. Open Questions

| Mã | Câu hỏi mở | Tác động |
| :-- | :-- | :-- |
| SEC-OPEN-001 | Bắt buộc SSO hay chỉ email/mật khẩu ở MVP? | Ảnh hưởng auth/API/UI. |
| SEC-OPEN-002 | Mã hoá at-rest ở mức nào? | Ảnh hưởng DB và vận hành. |

## 11. Tham chiếu

- [`01-srs.md`](01-srs.md)
- [`04-database-design.md`](04-database-design.md)
- [`05-api-specification.md`](05-api-specification.md)
- STRIDE