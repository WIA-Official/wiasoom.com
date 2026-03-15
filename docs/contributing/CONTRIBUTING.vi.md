<p align="center">
  <img src="https://wiasoom.com/favicon.png" width="80" alt="WIA SOOM">
</p>

<h1 align="center">Đóng góp cho WIA SOOM</h1>
<p align="center"><strong>Chúng tôi rất mong nhận được sự đóng góp của bạn!</strong></p>
<p align="center">Dù là sửa lỗi, tính năng mới, plugin hay bản dịch — mọi đóng góp đều quan trọng.</p>

---

## Mục Lục

- [Quy tắc ứng xử](#code-of-conduct)
- [Cách báo cáo lỗi](#-how-to-report-bugs)
- [Cách đề xuất tính năng](#-how-to-suggest-features)
- [Cách gửi một plugin](#-how-to-submit-a-plugin)
- [Cách gửi một Pull Request](#-how-to-submit-a-pull-request)
- [Đóng góp bản dịch (254 ngôn ngữ)](#-translation-contributions-254-languages)
- [Cài đặt phát triển](#-development-setup)

---

## Quy tắc ứng xử

Chúng tôi cam kết cung cấp trải nghiệm chào đón và bao gồm cho mọi người.

- **Tôn trọng.** Đối xử với mọi người bằng sự tôn trọng.
- **Xây dựng.** Cung cấp phản hồi hữu ích, không phải chỉ trích phá hoại.
- **Bao gồm.** Chúng tôi hỗ trợ 254 ngôn ngữ và chào đón các nhà đóng góp từ mọi quốc gia trên Trái Đất.
- **Không quấy rối.** Không khoan nhượng cho bất kỳ hình thức phân biệt nào.

---

## 🐛 Cách báo cáo lỗi

1. Truy cập [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Nhấn **"New Issue"**
3. Chọn mẫu **"Bug Report"**
4. Bao gồm:
   - Phiên bản WIA SOOM (Cài đặt → Giới thiệu)
   - Hệ điều hành và phiên bản (Windows/macOS/Linux)
   - Các bước để tái tạo
   - Hành vi mong đợi so với hành vi thực tế
   - Ảnh chụp màn hình hoặc đầu ra terminal nếu có thể

---

## 💡 Cách đề xuất tính năng

1. Truy cập [GitHub Issues](https://github.com/WIA-Official/wiasoom.com/issues)
2. Nhấn **"New Issue"**
3. Chọn mẫu **"Feature Request"**
4. Mô tả:
   - Vấn đề bạn đang giải quyết
   - Cách bạn tưởng tượng nó hoạt động
   - Bất kỳ giải pháp thay thế nào bạn đã xem xét

---

## 🔌 Cách gửi một plugin

WIA SOOM có một hệ thống plugin mạnh mẽ — bạn có thể xây dựng plugin của riêng mình trong 5 phút.

### Bắt đầu nhanh
```bash
# Use the scaffold tool
./scripts/soom-create-plugin.sh
```
### Hướng dẫn đầy đủ

Đọc **[Hướng dẫn phát triển Plugin](docs/PLUGIN_DEVELOPER_GUIDE.md)** để:
- Tham khảo API đầy đủ
- Ví dụ hoạt động
- Hướng dẫn từng bước
- Thực hành tốt nhất và quy tắc bảo mật

### Gửi plugin của bạn

1. Fork [Plugin Store](https://wiasoom.com)
2. Thêm plugin của bạn vào `plugins/{tên-plugin-của-bạn}/`
3. Gửi một Pull Request
4. Sau khi xem xét, plugin của bạn sẽ xuất hiện trong Plugin Store cho tất cả người dùng!

---

## 🔀 Cách gửi một Pull Request

### Đối với ứng dụng chính (wia-soom)

1. Fork kho lưu trữ
2. Tạo một nhánh tính năng: `git checkout -b feat/my-feature`
3. Thực hiện các thay đổi của bạn
4. Kiểm tra cục bộ:
   ```bash
   ```
5. Cam kết với một thông điệp rõ ràng:
   ```
   feat: thêm công tắc chế độ tối vào cài đặt
   ```
6. Đẩy và mở một PR chống lại `main`

### Quy tắc thông điệp cam kết

| Tiền tố | Sử dụng cho |
|---------|-------------|
| `feat:` | Tính năng mới |
| `fix:`  | Sửa lỗi |
| `docs:` | Chỉ tài liệu |
| `refactor:` | Tái cấu trúc mã (không thay đổi hành vi) |
| `i18n:` | Cập nhật bản dịch |
| `plugin:` | Thay đổi liên quan đến plugin |

### Danh sách kiểm tra PR

- [ ] Mã chạy mà không có lỗi
- [ ] Không có chuỗi cứng (sử dụng khóa i18n)
- [ ] Không có `console.log` còn lại trong mã sản xuất
- [ ] Các bài kiểm tra hiện có vẫn hoạt động

---

## 🌐 Đóng góp bản dịch (254 ngôn ngữ)

WIA SOOM hỗ trợ **254 ngôn ngữ** — từ Amharic đến Zulu, bao gồm cả chữ nổi và các ngôn ngữ RTL.

### Cách hoạt động của bản dịch

- Tệp ngôn ngữ cơ bản: `src/renderer/src/i18n/en.json`
- Tất cả 254 tệp ngôn ngữ đều ở cùng một thư mục
- Bản dịch được thực hiện thông qua `scripts/translate-patch.js` (GPT-4o-mini API)

### Cách đóng góp bản dịch

#### Tùy chọn 1: Sửa một bản dịch cụ thể

1. Tìm tệp ngôn ngữ: `src/renderer/src/i18n/{mã-ngôn-ngữ}.json`
2. Sửa bản dịch không chính xác
3. Gửi một PR với thay đổi

#### Tùy chọn 2: Thêm các khóa còn thiếu
```bash
# Sync all languages with new English keys
npm run i18n:sync

# Translate only new/missing keys
node scripts/translate-patch.js
```
#### Tùy chọn 3: Xem xét các bản dịch máy

Nhiều ngôn ngữ trong số 254 ngôn ngữ của chúng tôi đã được dịch bằng máy. Các đánh giá từ người bản ngữ là vô cùng quý giá!

1. Chọn tệp ngôn ngữ của bạn
2. Xem xét các bản dịch
3. Sửa bất kỳ bản dịch nào không tự nhiên hoặc không chính xác
4. Gửi một PR

### Mã ngôn ngữ

Chúng tôi sử dụng mã tiêu chuẩn ISO 639-1 (ví dụ: `ko`, `en`, `ja`, `ar`, `hi`) với các biến thể khu vực khi cần (ví dụ: `zh-CN`, `pt-BR`).

---

## 🛠 Cài đặt phát triển

### Điều kiện tiên quyết

- Node.js 18+
- npm 9+
- Git

### Cài đặt
```bash
```
### Xây dựng
```bash
```
> Lưu ý: Bộ nhớ heap mặc định 2GB không đủ do 254 tệp ngôn ngữ + gói biên dịch Monaco (~38MB).

### Cấu trúc dự án
```
wia-soom/
├── src/
│   ├── main/          # Electron main process
│   ├── renderer/      # React frontend
│   └── preload/       # Preload scripts
├── docs/              # Documentation
├── scripts/           # Build & automation scripts
└── prompts/           # AI prompt engineering
```
---

## 🙏 Cảm Ơn

Mỗi đóng góp đều giúp WIA SOOM trở nên tốt hơn cho các nhà phát triển trên toàn thế giới.

Dù bạn sửa một lỗi chính tả, dịch một chuỗi, xây dựng một plugin, hay thêm một tính năng lớn — **bạn là một phần của câu chuyện này.**

---

<p align="center"><em>Xây dựng với ❤️ bởi SmileStory Inc. và các đóng góp viên trên toàn thế giới.</em></p>