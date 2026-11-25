# C2C Escrow Digital Marketplace — Case Study về Product, BA & BI

Một Case Study về **Phân tích nghiệp vụ (BA), Quản lý sản phẩm/dự án (PO/PM) và Business Intelligence (BI)** cho nền tảng thương mại điện tử C2C chuyên giao dịch tài sản số (mã nguồn, UI/UX kit, tài liệu học thuật và biểu mẫu). Hệ thống giải quyết triệt để rủi ro lừa đảo trong giao dịch ngang hàng (P2P), tình trạng đánh cắp bản quyền và nội dung AI thiếu minh bạch thông qua ví thanh toán trung gian khép kín (Escrow), cơ chế đóng dấu bản quyền tự động và quy trình kiểm duyệt minh bạch.

---

## 1. Tổng quan & Bài toán nghiệp vụ (Problem Statement)

Mô hình giao dịch tài liệu số P2P tự phát trên mạng xã hội và diễn đàn hiện nay bộc lộ nhiều rủi ro vận hành cho cả người mua lẫn người bán:
- **Rủi ro tài chính & dòng tiền:** Chuyển khoản trực tiếp thiếu cơ chế bảo vệ trung gian, dễ dẫn đến lừa đảo (không nhận được tệp hoặc tài liệu sai mô tả) và khó kiểm soát doanh thu.
- **Đánh cắp sở hữu trí tuệ:** Các tệp xem trước (preview) không được bảo vệ dễ bị chụp màn hình hoặc tải lậu khi chưa thanh toán.
- **Nội dung AI thiếu minh bạch:** Việc lạm dụng các mô hình ngôn ngữ lớn (LLM) để tạo tài liệu hàng loạt làm giảm chất lượng sàn và mất niềm tin của người mua.

**Giải pháp đề xuất:** Xây dựng nền tảng trung gian C2C chuẩn hóa tích hợp **Ví điện tử nội bộ chuẩn ACID**, cơ chế **Đóng dấu bản quyền chéo góc (Diagonal Watermarking)** tự động, động cơ **Phân tích nhãn AI cục bộ (NLP Heuristics)** và quy trình **Kiểm duyệt & Xử lý tranh chấp** chặt chẽ.

---

## 2. Vai trò & Trách nhiệm (Role & Responsibilities)

* **Business Analyst (BA):** Ứng dụng nền tảng phương pháp luận từ **BABOK Guide v3** để xác định phạm vi dự án (In-Scope/Out-Scope), khơi gợi yêu cầu trên 4 tác nhân, xây dựng tài liệu đặc tả yêu cầu phần mềm (SRS) gồm **46 Use Cases** và **8 Quy tắc nghiệp vụ (Business Rules)**, thiết kế bộ **Sơ đồ UML** (Use Case, Activity, Sequence), **ERD** và thực hiện kiểm thử chấp nhận người dùng (UAT).
* **Product Owner / Điều phối dự án (PO/PM):** Phân rã và sắp xếp mức độ ưu tiên cho 8 phân hệ chức năng theo lộ trình sản phẩm, định nghĩa tiêu chí nghiệm thu (Acceptance Criteria) cho các luồng tài chính/kiểm duyệt lõi và điều phối tiến độ trong nhóm 5 thành viên.
* **Business Intelligence (BI):** Thiết kế các khung nhìn dữ liệu (`vw_topproducts`, `vw_seller_revenue`) và hệ thống chỉ số KPI nhằm giám sát Tổng giá trị giao dịch (GMV), doanh thu phí hoa hồng sàn, dòng tiền đóng băng chờ giải ngân và tỷ lệ phân bổ nhãn AI.

---

## 3. Phạm vi hệ thống & Tác nhân (System Scope & Actors)

### Các tác nhân hệ thống
1. **Khách vãng lai / Người mua (Guest / Buyer - Role 1):** Tìm kiếm, lọc sản phẩm, quản lý giỏ hàng (Session/DB), nạp tiền vào ví nội bộ, thanh toán đơn lẻ/gộp giỏ hàng, tải tài liệu an toàn, đánh giá, yêu cầu hoàn tiền và tố cáo đạo nhái.
2. **Người bán / Nhà sáng tạo (Seller / Creator - Role 2):** Đăng ký gian hàng với URL slug riêng, đăng tải sản phẩm số, xem kết quả quét nhãn AI, gửi khiếu nại nhãn AI, theo dõi doanh thu và tạo lệnh rút tiền về ngân hàng.
3. **Quản trị viên (Admin - Role 3):** Kiểm duyệt chất lượng sản phẩm, cấu hình tỷ lệ phí hoa hồng sàn động, thẩm định hồ sơ eKYC, phân xử tố cáo bản quyền/khiếu nại AI và phê duyệt giải ngân rút tiền.
4. **Đối tác Cổng thanh toán (External):** Hỗ trợ mô phỏng luồng nạp tiền và đối soát giải ngân.

### 8 Phân hệ chức năng (46 Use Cases)
| Phân hệ | Tóm tắt phạm vi nghiệp vụ |
| :--- | :--- |
| **1. Xác thực & Định danh (eKYC)** | Đăng ký kèm tự động tạo ví, mã hóa Bcrypt, khôi phục mật khẩu SMTP, nộp và duyệt eKYC. |
| **2. Quản lý Gian hàng** | Nâng cấp tài khoản Seller, cấu hình thông tin cửa hàng/ngân hàng, trang gian hàng công khai. |
| **3. Sản phẩm & Bản quyền** | Tải tệp (ZIP/PDF/RAR), tự động đóng dấu Watermark (GD/FPDF), quét nhãn AI, khiếu nại nhãn AI. |
| **4. Khám phá & Tương tác** | Bộ lọc đa tiêu chí, lưu yêu thích (AJAX), đánh giá 5 sao, bình luận phân cấp. |
| **5. Giỏ hàng & Thanh toán** | Tự động hợp nhất giỏ hàng tạm (Guest Cart), thanh toán đơn lẻ và gộp giỏ hàng chuẩn ACID. |
| **6. Ví nội bộ & Giải ngân** | Quản lý số dư khả dụng và đóng băng, tải tệp an toàn có lưu vết IP, tạo lệnh rút tiền. |
| **7. Tranh chấp & Hoàn tiền** | Tố cáo đạo nhái, khiếu nại chất lượng, cơ chế hoàn tiền tự động trong vòng 7 ngày. |
| **8. Quản trị & Vận hành** | Thống kê toàn sàn, khóa tài khoản vi phạm, nhật ký kiểm duyệt (Audit Log), duyệt rút tiền, cấu hình động. |

---

## 4. Luồng nghiệp vụ lõi & Quy tắc hệ thống

### Các luồng vận hành trọng tâm
- **Thanh toán giỏ hàng nguyên tử (ACID):** Khóa dòng dữ liệu ví người mua và người bán bằng `SELECT ... FOR UPDATE`, trừ tổng tiền giỏ hàng, tự động trích phí hoa hồng sàn (mặc định 5%), kết chuyển doanh thu ròng cho từng người bán và mở quyền tải tệp trong cùng một Transaction.
- **Vòng đời Giải ngân 2 bước:** Khi người bán tạo lệnh rút tiền, Stored Procedure `sp_RequestWithdrawal` chuyển tiền từ `balance` sang `frozen_balance`. Sau khi đối soát chuyển khoản thực tế, Admin thực thi `sp_ApproveWithdrawal` để khấu trừ vĩnh viễn số dư đóng băng (hoặc từ chối để hoàn tiền lại ví).
- **Kiểm soát nội dung tự động:** Ảnh và PDF xem trước được đóng dấu mờ chéo góc; nội dung mô tả được chấm điểm bằng NLP Heuristics (mẫu câu LLM, Burstiness, TTR) để gán nhãn minh bạch (`Human Written`, `Mixed`, `AI Generated`).
- **Hoàn tiền 7 ngày & Thu hồi quyền tải:** Đơn hàng hợp lệ (`status = 2`, trong vòng 7 ngày) kích hoạt `RefundService` để hoàn tiền về ví người mua, thu hồi doanh thu từ ví người bán và lập tức khóa quyền tải tệp gốc.

### Quy tắc nghiệp vụ cốt lõi (Business Rules)
| Mã quy tắc | Tên quy tắc | Đặc tả chi tiết |
| :--- | :--- | :--- |
| **BR-01** | Cấm tự mua hàng | Người bán không được phép mua sản phẩm của chính mình (`seller_id != buyer_id`). |
| **BR-02** | Tính duy nhất trong giỏ | Mỗi tài sản số chỉ xuất hiện 1 lần trong giỏ hàng (không áp dụng số lượng). |
| **BR-03** | Phí hoa hồng động | `seller_amount = price - (price * commission_rate / 100)` cấu hình động qua bảng `settings`. |
| **BR-04** | Ngưỡng phân loại AI | `<= 40%`: Human Written; `40% - 70%`: Mixed; `> 70%`: AI Generated. |
| **BR-05** | Đóng băng quỹ rút tiền | Số tiền yêu cầu rút bị khóa trong `frozen_balance` cho đến khi Admin xử lý xong. |
| **BR-06** | Thời hạn hoàn tiền | Người mua chỉ được yêu cầu hoàn tiền cho đơn đã thanh toán trong vòng 7 ngày. |
| **BR-07** | Bảo vệ tài khoản Admin | Tài khoản Quản trị viên (`role = 3`) không thể bị khóa hoặc xóa cứng qua giao diện. |
| **BR-08** | Thời hạn Token mật khẩu | Token khôi phục mật khẩu hết hạn sau 15 phút và bị xóa ngay sau khi sử dụng. |

---

## 5. Yêu cầu phi chức năng & Kiến trúc hệ thống

- **Bảo mật (Security):** Xác thực CSRF Token (`hash_equals`), cấu hình Cookie `HttpOnly` & `SameSite=Lax`, tái tạo phiên `session_regenerate_id`, mã hóa mật khẩu Bcrypt, 100% truy vấn dùng PDO Prepared Statements, kiểm tra MIME-type/dung lượng tệp tải lên và kiểm soát tải tệp qua Controller trung gian.
- **Toàn vẹn dữ liệu (Data Integrity):** MySQL 8.0+ (InnoDB) kết hợp `beginTransaction`, `commit`, `rollBack` và khóa dòng `SELECT ... FOR UPDATE` để chống lỗi trừ tiền hai lần (Double Spending) và xung đột đồng thời (Race Condition).
- **Hiệu năng (Performance):** Kết nối PDO bền vững (Persistent Connection), cơ chế Query Caching tại tầng Model và động cơ NLP Heuristics chạy cục bộ trên RAM (thời gian phản hồi < 50ms).

---

## 6. Tầng Phân tích dữ liệu & Business Intelligence (BI)

Xây dựng trên lược đồ cơ sở dữ liệu quan hệ (`users`, `wallets`, `transactions`, `products`, `documents`, `orders`, `withdraw_requests`) cùng các Database View (`vw_topproducts`, `vw_seller_revenue`), tầng phân tích tập trung theo dõi:
- **Sức khỏe tài chính:** Tổng giá trị giao dịch (GMV), Doanh thu ròng từ phí hoa hồng sàn, Tổng số dư đóng băng chờ giải ngân và Tỷ lệ hoàn tiền (Refund Rate).
- **Hiệu quả vận hành sàn:** Tỷ lệ chuyển đổi người bán, Nhóm danh mục mang lại doanh thu cao nhất và Thời gian trung bình duyệt sản phẩm.
- **Chất lượng nội dung & Rủi ro:** Tỷ lệ phân bổ nhãn AI (`ai_score`), Tỷ lệ khiếu nại AI được chấp thuận và Tỷ lệ xử lý báo cáo đạo nhái.

---

## 7. Lộ trình phát triển sản phẩm (Product Roadmap)

- **Giai đoạn 1 — MVP (Đã hoàn thành):** Sàn C2C lõi, kiến trúc PHP MVC thuần, ví nội bộ mô phỏng, thanh toán/hoàn tiền chuẩn ACID, đóng dấu Watermark tự động và phân tích AI cục bộ.
- **Giai đoạn 2 — Thanh toán & Tự động hóa (Kế hoạch):** Tích hợp cổng thanh toán thực tế (VNPay, MoMo, VietQR) để tự động hóa nạp tiền và đối soát ngân hàng.
- **Giai đoạn 3 — Mở rộng & Tin cậy (Kế hoạch):** Phát triển ứng dụng di động (Mobile App), nâng cấp mô hình Machine Learning phát hiện nội dung AI và chứng thực quyền sở hữu số.

---

## 8. Kiểm thử chất lượng (Tóm tắt UAT)

Các kịch bản nghiệp vụ lõi đã được kiểm thử hộp đen (Black-box Testing) và đạt chuẩn:
- `TC-01`: Đăng ký tài khoản mới mã hóa Bcrypt và tự động khởi tạo ví số dư 0 — **Đạt**
- `TC-02`: Đăng bán sản phẩm và tự động đóng dấu Watermark chéo góc lên ảnh/PDF — **Đạt**
- `TC-03`: Phân tích NLP Heuristics và gán nhãn `AI Generated` (`> 70%`) tự động — **Đạt**
- `TC-04`: Thanh toán gộp giỏ hàng chuẩn ACID, trích 5% phí sàn và cộng tiền nhiều Seller — **Đạt**
- `TC-05`: Phong tỏa quỹ rút tiền (`sp_RequestWithdrawal`) và phê duyệt giải ngân (`sp_ApproveWithdrawal`) — **Đạt**
- `TC-06`: Xử lý hoàn tiền tự động trong 7 ngày và thu hồi ngay quyền tải tệp gốc — **Đạt**

---

## 9. Cấu trúc thư mục & Liên kết

```text
C2C-Escrow-Marketplace/
├── docs/                   # Tài liệu SRS, Business Rules, UAT Test Cases và Sơ đồ UML/ERD
├── analytics/              # Truy vấn SQL, định nghĩa KPI và Power BI Dashboard (.pbix)
└── README.md               # Tổng quan Case Study về Product, BA & BI
