# Sổ Kho — web app nhập xuất kho

App quản lý kho chạy trên điện thoại (thêm được vào màn hình chính như app).
Dữ liệu lưu trong Google Sheet **So-Kho-v2**, ảnh lưu trong Google Drive › **Kho hàng**.

- `index.html`, `manifest.webmanifest`, `sw.js`, `icon-*.png`: giao diện, đưa lên GitHub Pages
- `apps-script/WebApp.gs`: phần "máy chủ", dán vào Apps Script của Google Sheet
- `apps-script/Code.gs`, `apps-script/Sidebar.html`: menu Sổ Kho + form bên trong Google Sheet (bản đang dùng)

---

## Bước 1 — Bật API trong Google Sheet (làm 1 lần, ~5 phút)

1. Mở Google Sheet **So-Kho-v2** → **Tiện ích mở rộng → Apps Script**.
2. Bấm dấu **+** cạnh "Tệp" → **Tập lệnh** → đặt tên `WebApp` → dán toàn bộ nội dung `apps-script/WebApp.gs`.
3. Sửa dòng `var WEB_PIN = '1234';` thành mã PIN riêng của bạn. **Chỉ sửa trong Apps Script, không sửa file trên GitHub.**
4. Bấm **Lưu**, chọn hàm `testWebApp` → **Chạy** → cho phép quyền truy cập Sheets và Drive.
5. **Triển khai → Tùy chọn triển khai mới** → biểu tượng bánh răng → **Ứng dụng web**:
   - Thực thi dưới dạng: **Tôi**
   - Người có quyền truy cập: **Bất kỳ ai**
6. Bấm **Triển khai** → sao chép **URL ứng dụng web** (kết thúc bằng `/exec`).

> Khi sửa code Apps Script sau này: **Triển khai → Quản lý các lần triển khai → ✏ → Phiên bản: Phiên bản mới**. Làm như vậy thì link `/exec` giữ nguyên.

## Bước 2 — Đưa lên GitHub Pages

1. Vào github.com → **New repository** → tên `so-kho` → **Public** → Create.
2. Bấm **uploading an existing file** → kéo thả **tất cả file trong thư mục này** (kể cả thư mục `apps-script`) → **Commit changes**.
3. **Settings → Pages** → Source: **Deploy from a branch** → Branch: `main` / `(root)` → **Save**.
4. Sau 1–2 phút, app chạy tại: `https://<tên-github-của-bạn>.github.io/so-kho/`

## Bước 3 — Dùng trên điện thoại

1. Mở link GitHub Pages bằng Chrome (Android) hoặc Safari (iPhone).
2. Bấm **⚙ Kết nối** → dán link `/exec` + mã PIN → **Kết nối**.
3. Thêm vào màn hình chính:
   - Android/Chrome: menu ⋮ → **Thêm vào màn hình chính**
   - iPhone/Safari: nút Chia sẻ → **Thêm vào MH chính**
4. Gửi cho nhân viên: link GitHub Pages + link `/exec` + mã PIN.

---

## Cách dữ liệu được lưu

| Trong app | Trong Google Sheet |
|---|---|
| Thêm mặt hàng | Dòng mới ở sheet **Hàng hóa** (cột A–H). Ảnh: cột **P** |
| Nhập / Xuất | Dòng mới ở sheet **Nhập Xuất** (Ngày, Loại, Mã, Số lượng, Ghi chú). Ảnh: cột **J** |
| Tồn kho, trạng thái | Do công thức trong Sheet tự tính, app chỉ đọc lên |
| Ảnh | File JPG trong Drive › Kho hàng, tên dạng `2026-09-24_160512_NHAP_BAG_Tui-cho-KTV.jpg` |

## Lưu ý

- **Bảo mật:** ai có đủ link `/exec` và mã PIN thì ghi sổ được. Link `/exec` và PIN không nằm trong code trên GitHub; mỗi máy tự nhập và chỉ lưu trên máy đó.
- **Ảnh:** app đặt ảnh ở chế độ "Bất kỳ ai có đường liên kết đều xem được" để hiển thị được trên web. Người ngoài không tìm thấy ảnh nếu không có link.
- **Mã hàng không đổi được** sau khi tạo, để lịch sử nhập xuất không bị lệch. Xóa mặt hàng trong app chỉ xóa nội dung dòng, công thức vẫn giữ nguyên.
- App tự làm mới dữ liệu mỗi 30 giây, và mỗi lần bạn mở lại app.
- Chưa kết nối thì app hiện **dữ liệu mẫu** để xem thử; dữ liệu mẫu không ghi vào Sheet.
