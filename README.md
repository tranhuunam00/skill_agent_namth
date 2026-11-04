# 🚀 Bộ Kỹ Năng Agentic AI (Antigravity Skills Collection) - NamTH

Kho lưu trữ tổng hợp các **Kỹ Năng Tự Động Hóa Chuyên Sâu (Agent Skills)** được thiết kế và chuẩn hóa cho hệ sinh thái **Google Antigravity IDE / Gemini Agent**.

---

## 📦 Danh Mục Kỹ Năng (Skills Catalog)

### 1. 📄 `pdf-to-word-converter`
**Chuyên gia chuyển đổi biểu mẫu, tài liệu từ PDF sang Word (DOCX) chuẩn thể thức hành chính Việt Nam (Nghị định 30/2020/NĐ-CP) và độ chính xác 1:1.**
- **Tính năng nổi bật**:
  - Tái tạo bảng biểu phức tạp (Rowspan / Colspan) chuẩn OpenXML, không bao giờ bị xô lệch cột trên MS Word & Google Docs.
  - Xử lý cơ chế **Footnote OpenXML** (`word/footnotes.xml`) tự động hiển thị phân cách chân trang chuẩn xác.
  - Thiết lập Section **Khổ ngang (Landscape A4: 297mm × 210mm)** cho các bảng dự toán tài chính nhiều cột.
  - Tích hợp **Google Drive & Google Docs API** qua Service Account tự động cập nhật văn bản lên cloud.
  - Pipeline **Kiểm thử thị giác (Visual QC)**: Word COM -> PDF -> Render ảnh 200 DPI đối soát 1:1.

---

### 2. 📝 `docx`
**Bộ công cụ toàn diện để tạo, đọc, sửa đổi và xử lý tài liệu Word (.docx) và template (.dotx).**
- **Tính năng nổi bật**:
  - Hỗ trợ thao tác sâu với cấu trúc OpenXML ECMA-376 / ISO-IEC 29500.
  - Thêm, sửa, duyệt comment và Track Changes (Redlining).
  - Quản lý metadata, styles, typography và XML validation.

---

### 3. 🛡️ `web-security-auditor`
**Chuyên gia rà soát, kiểm thử và khắc phục toàn diện các lỗ hổng bảo mật cho ứng dụng Web Fullstack (Node.js/Express, SQLite, REST API, SPA) đạt chuẩn OWASP Top 10, bảo mật dữ liệu y tế (Nghị định 13/2023/NĐ-CP) và yêu cầu thẩm định an ninh mạng doanh nghiệp (Vin IT / Vingroup InfoSec).**
- **Tính năng nổi bật**:
  - Tự động quét và phòng thủ 105+ kịch bản tấn công: SQL Injection (Prepared Statements), Stored XSS, JWT Token Tampering, IDOR & RBAC, Path Traversal, Prototype Pollution.
  - Tích hợp HTTP Security Headers (`helmet`), Anti-DDoS & Rate Limiting (`express-rate-limit`).
  - Chuẩn hóa mã hóa mật khẩu Bcrypt 10 Salt Rounds, HttpOnly Secure Cookie, cô lập Secret 256-bit trong `.env`.
  - Bộ kiểm thử an ninh tự động hóa (Automated Security Audit Suite).

---

### 4. 📐 `code-convention`
**Quy chuẩn mã nguồn (Code Convention & Style Guide) chuẩn doanh nghiệp kết hợp TDD & DDD.**
- **Tính năng nổi bật**:
  - Bắt buộc kiểm thử 100% mọi hàm mới (Function Testing Mandate) với ma trận 4D: Input, Output, Error (ném ngoại lệ, mã lỗi), và Logic.
  - Thiết kế hướng miền (DDD): Ubiquitous Language, Value Objects bất biến (Self-Validating), Aggregate Roots, Domain Services.
  - Hệ thống quy ước đặt tên (Naming Conventions) chặt chẽ cho biến, hàm, class, enum, DB, API routes.
  - Clean Architecture, Guard Clauses, loại bỏ hoàn toàn Magic Numbers.
  - Chuẩn Git Conventional Commits (`feat:`, `fix:`, `refactor:`,...) và Definition of Done (DoD).

---

### 5. 🎨 `fe-development`
**Quy chuẩn phát triển Frontend & Mobile UI chuẩn doanh nghiệp: module hóa component, quản lý state chặt chẽ, đồng bộ Design Tokens và tối ưu UX tiết kiệm không gian.**
- **Tính năng nổi bật**:
  - Tách nhỏ component tái sử dụng cao theo Atomic Design (Atoms, Molecules, Organisms, Screens); tách Dumb UI và Smart Hooks.
  - Quản lý state màn hình chặt chẽ theo State Machine: 5 trạng thái (`Loading`, `Empty`, `Error`, `Data`, `Offline Sync`).
  - Đồng bộ màu sắc, typography và spacing lưới 4px qua hệ thống Design Tokens (Primary Teal `#1F7A8C`, Card bo tròn 22px).
  - Tối ưu không gian hiển thị: Cuộn nội bộ trong khung (In-Frame Internal Scroll max 5 items), Carousel Chip filter, Modal Bottom Sheet.
  - Trải nghiệm công thái học (Ergonomics): Touch target ≥ 44 × 44 pt, tối ưu Thumb Zone, tương phản chữ ≥ 4.5:1 (WCAG 2.1 AA).

---

### 6. 📋 `spec-kit`
**Phương pháp luận Specification-Driven Development (SDD) từ GitHub cho AI Coding Agents: Code phục vụ Đặc tả, bắt buộc kiểm thử TDD, Kế hoạch kiến trúc (Plan), Thực thể CSDL (Data Model) và Danh sách tác vụ (Tasks).**
- **Tính năng nổi bật**:
  - Đảo chiều phát triển: Code phục vụ Đặc tả (Specifications), không code khi chưa có Spec được phê duyệt.
  - Thiết lập Hiến pháp kỹ thuật (`constitution.md`) với các Cổng kiểm soát (Constitution Check Gates).
  - Quản trị tài liệu dự án chuẩn hóa theo vòng đời: `spec.md` $\rightarrow$ `plan.md` $\rightarrow$ `data-model.md` $\rightarrow$ `tasks.md` $\rightarrow$ `quickstart.md`.
  - Phân rã tác vụ theo User Stories ưu tiên (P1, P2, P3) kèm gắn cờ song song `[P]`.
  - Liên kết chặt chẽ với quy chuẩn TDD và Clean Architecture.

---

## 🛠️ Hướng Dẫn Cài Đặt & Sử Dụng

### Cách 1: Sử dụng trong từng Workspace cụ thể
Sao chép thư mục `.agents` vào thư mục gốc của dự án:
```bash
# Trong thư mục dự án của bạn
mkdir -p .agents
cp -r /path/to/skill_agent_namth/skills .agents/
```

### Cách 2: Sử dụng Toàn Cầu (Global Skills)
Sao chép thư mục `skills` vào cấu hình Global của Antigravity:
- **Windows**: `C:\Users\<Username>\.gemini\config\skills\`
- **Linux / macOS**: `~/.gemini/config/skills/`

---

## 👨‍💻 Tác Giả & Bản Quyền
- **Maintainer**: NamTH (`tranhuunam00`)
- **Hệ thống**: Antigravity Agentic Customization System
