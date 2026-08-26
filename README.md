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
