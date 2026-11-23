# CLAUDE.md

Tài liệu này tóm tắt các trang đã xây dựng (`login.html`, `register.html`) và quy định các quy tắc thiết kế chung cho mọi trang cơ bản trong tương lai của dự án.

## 1. Tổng quan dự án

- Dự án gồm các trang HTML tĩnh, một file cho mỗi trang, đặt ở thư mục gốc.
- Công nghệ: HTML + Tailwind CSS (CDN) + Lucide Icons (CDN) + JavaScript thuần (không framework, không bước build).
- Thương hiệu tạm dùng trong giao diện: **Ember** (logo `flame` của Lucide trên nền cam).
- Nội dung giao diện: **100% tiếng Anh** (nhãn, placeholder, thông báo lỗi, nút, liên kết). Tài liệu này viết bằng tiếng Việt.

## 2. Các trang đã tạo

### 2.1 `login.html` – Đăng nhập

**Mục đích:** cho người dùng hiện có đăng nhập vào hệ thống.

**Yêu cầu:**
- Trường Email và Password.
- Nút Sign in.
- Checkbox "Remember me".
- Liên kết "Forgot password?".
- Đăng nhập nhanh bằng Google / GitHub / Facebook.
- Liên kết sang trang đăng ký: "Don't have an account yet? Please sign up" → `register.html`.

**Hành động:**
- Nút hiện/ẩn mật khẩu (`eye` / `eye-off`).
- Validate khi submit: email bắt buộc và đúng định dạng, password bắt buộc. Lỗi hiện ngay dưới trường, focus chuyển đến trường lỗi đầu tiên; lỗi tự cập nhật khi người dùng sửa.
- Khi hợp lệ: hiện chỉ ghi dữ liệu ra `console.log` (chỗ gắn API đăng nhập thật được đánh dấu bằng comment trong script).

**Kết quả:** trang đăng nhập hoàn chỉnh, chia đôi màn hình trên desktop (panel thương hiệu bên trái, form bên phải), một thẻ căn giữa trên tablet/mobile.

### 2.2 `register.html` – Đăng ký

**Mục đích:** tạo tài khoản mới.

**Các trường (theo thứ tự hiển thị):**

| Trường | Bắt buộc | Ràng buộc |
| --- | --- | --- |
| Full name | Có | Không được để trống |
| Email | Có | Đúng định dạng email |
| Phone number | Không | Chỉ nhận chữ số; nếu nhập thì phải đúng 10 số |
| Password | Có | Tối thiểu 8 ký tự |
| Confirm password | Có | Phải trùng với Password |
| Gender | Không | Male / Female / Prefer not to say |
| Address | Không | Văn bản tự do |
| Terms checkbox | Có | Phải được chọn (kèm liên kết Terms of Service và Privacy Policy) |

**Hành động:**
- Nút hiện/ẩn mật khẩu cho cả Password và Confirm password.
- Ô Phone tự loại ký tự không phải số và giới hạn 10 ký tự.
- Validate khi submit; sau khi một trường đã báo lỗi thì kiểm tra lại ngay khi nhập. Sửa Password sẽ kiểm tra lại Confirm password.
- Liên kết quay lại: "Back to sign in" → `login.html` (logo cũng trỏ về `login.html`).
- Khi hợp lệ: hiện chỉ ghi dữ liệu ra `console.log` (chỗ gắn API đăng ký thật được đánh dấu bằng comment).

**Kết quả:** form đăng ký responsive; panel thương hiệu bên trái cố định (sticky) khi form dài cuộn trên desktop.

### 2.3 Liên kết giữa hai trang

- `login.html` → `register.html` (link "Please sign up").
- `register.html` → `login.html` (link "Back to sign in" và logo).

## 3. Quy tắc thiết kế chung (áp dụng cho mọi trang mới)

### 3.1 Theme màu sắc và font chữ (bắt buộc dùng chung)

Chỉ dùng **dark theme** trên toàn bộ trang. Khai báo token Tailwind giống hệt các trang hiện có:

| Token | Giá trị | Vai trò |
| --- | --- | --- |
| `ink` | `#090A14` | Nền trang |
| `surface` | `#11131F` | Nền input, thẻ, nút phụ |
| `line` | `#23263A` | Viền, đường phân cách |
| `muted` | `#9A9DB5` | Văn bản phụ, icon, placeholder |
| `primary` | `#DF6B33` | Màu chính / CTA |
| `primary-hover` | `#E97E4A` | Trạng thái hover của màu chính |
| Lỗi | `red-400` (Tailwind) | Thông báo lỗi và viền lỗi |

- **Poppins** 700–800 cho tiêu đề, nút CTA và logo (`font-heading`).
- **Inter** 400 cho nội dung (`font-body`).
- Nút/CTA nền `primary` dùng **chữ màu `ink`** (không dùng chữ trắng, vì độ tương phản của trắng trên `#DF6B33` không đủ).
- Không đưa màu hoặc font mới vào nếu chưa có yêu cầu; không dùng light mode.
- Cấu hình Tailwind, `<link>` Google Fonts và CSS nền (`html { background: #090A14 }`) copy từ `login.html`.

### 3.2 Bố cục và responsive

- Thiết kế **mobile-first**, hoạt động tốt trên PC, tablet và điện thoại (kiểm tra tối thiểu ở 360px, 768px, 1280px).
- Có thẻ `<meta name="viewport" content="width=device-width, initial-scale=1">`.
- Trang xác thực dùng bố cục chuẩn:
  - `lg` (≥1024px): lưới 2 cột `5fr / 6fr`, panel thương hiệu bên trái (ẩn dưới `lg`), form bên phải.
  - Dưới `lg`: một cột, thẻ form căn giữa, logo hiển thị phía trên form.
  - Form dài: panel trái `sticky top-0 h-screen`.
- Trường ghép cặp (email + phone, password + confirm) xếp 2 cột từ `sm` trở lên, 1 cột trên điện thoại.
- Kích thước chữ input ≥ `text-base` để iOS không tự phóng to khi focus.
- Tôn trọng `prefers-reduced-motion` (tắt animation/transition).
- Chuyển động tự động tối thiểu: chỉ một hiệu ứng `rise` khi tải trang.

### 3.3 Lucide Icons

- Dùng Lucide cho mọi icon giao diện, nạp từ `https://unpkg.com/lucide@latest` và gọi `lucide.createIcons()` sau khi tải trang.
- Dùng cú pháp `<i data-lucide="tên-icon" class="h-5 w-5"></i>`.
- Sau khi đổi icon bằng JS (ví dụ `eye` ↔ `eye-off`), phải gọi lại `lucide.createIcons()`.
- Mỗi input có icon dẫn đầu (`mail`, `lock`, `lock-keyhole`, `user`, `phone`, `map-pin`...), đặt `pointer-events-none` và màu `text-muted`.
- Nút có icon đi kèm chữ (`log-in`, `user-plus`, `arrow-left`...).
- Lucide không có logo thương hiệu, nên logo Google/GitHub/Facebook dùng **SVG inline**.
- Icon thuần trang trí thêm `aria-hidden`; nút chỉ có icon phải có `aria-label`.

### 3.4 Ràng buộc nhập liệu

- Mọi form dùng `novalidate` và tự validate bằng JS; thông báo lỗi bằng tiếng Anh, nói rõ lỗi gì và cách sửa (ví dụ "Enter a valid email address, like you@example.com.").
- Trường bắt buộc đánh dấu `*` trong nhãn và có thuộc tính `required`.
- Quy tắc tối thiểu cho các trường quan trọng:
  - **Email:** bắt buộc, đúng định dạng `^[^\s@]+@[^\s@]+\.[^\s@]+$`.
  - **Password:** bắt buộc; khi tạo mới tối thiểu 8 ký tự.
  - **Confirm password:** phải trùng Password.
  - **Phone:** nếu có thì đúng 10 chữ số, chỉ nhận số (`inputmode="numeric"`, `maxlength="10"`).
  - **Checkbox điều khoản:** bắt buộc chọn.
  - Trường tùy chọn **không** báo lỗi khi để trống.
- Đặt đúng thuộc tính `type` và `autocomplete` (`email`, `current-password`, `new-password`, `name`, `tel`...).
- Hiển thị lỗi: viền `red-400`, thông báo `<p role="alert">` ngay dưới trường, liên kết với input bằng `aria-describedby`, đánh dấu `aria-invalid="true"`.
- Khi submit lỗi: focus vào trường lỗi đầu tiên; sau khi trường đã có lỗi thì validate lại theo thời gian thực khi người dùng nhập.
- Nút hiện/ẩn mật khẩu cho mọi trường mật khẩu, kèm `aria-label` và `aria-pressed`.

### 3.5 Liên kết chuyển đổi giữa các trang liên quan

- Mọi trang thuộc một luồng phải có liên kết hai chiều tới trang liên quan (đăng nhập ↔ đăng ký, đăng nhập → quên mật khẩu → quay lại đăng nhập...).
- Liên kết "tiến" dùng màu `primary` ở cuối form; liên kết "quay lại" dùng `text-muted` kèm icon `arrow-left`.
- Logo luôn là liên kết về trang chính của luồng.
- Dùng đường dẫn tương đối (`login.html`, `register.html`). Liên kết chưa có trang đích tạm dùng `href="#"`, và phải thay khi trang được tạo.
- Khi thêm trang mới, cập nhật liên kết ở các trang hiện có cho nhất quán.

### 3.6 Khả năng truy cập và chất lượng

- Mọi input có `<label for>` hiển thị; nhóm lựa chọn (radio) dùng `<fieldset>` + `<legend>`.
- Focus rõ ràng cho mọi phần tử tương tác (`focus-visible` hoặc `focus:ring`).
- Checkbox/radio tùy biến vẫn dùng input thật (`sr-only` + `peer`) để giữ bàn phím và trình đọc màn hình.
- Văn bản thông thường dùng chữ trắng hoặc `muted` trên nền `ink`/`surface`; không giảm tương phản dưới mức đọc được.
- Văn phong: động từ rõ ràng, sentence case, nút mô tả đúng hành động ("Sign in", "Create account").

## 4. Quy trình khi tạo trang mới

1. Copy phần `<head>` (fonts, Tailwind config, Lucide, CSS chung) từ `login.html`.
2. Dùng lại bố cục, class input/nút/thông báo lỗi đã có thay vì tự tạo kiểu mới.
3. Áp dụng các quy tắc 3.1–3.6.
4. Thêm liên kết hai chiều với các trang liên quan và cập nhật bảng bên dưới.
5. Kiểm tra: responsive ở 3 kích thước, validate từng trường, bàn phím (Tab/Enter), và không có lỗi trong console.

## 5. Danh sách trang

| File | Mô tả | Liên kết đến |
| --- | --- | --- |
| `index.html` | Trang giới thiệu công ty thương mại điện tử SHOPEX, sử dụng animate.style | `login.html`, `register.html` |
| `login.html` | Đăng nhập | `register.html` |
| `register.html` | Đăng ký | `login.html` |
| `charts.html` | Báo cáo thương mại điện tử (doanh thu, kết quả kinh doanh, marketing) bằng Chart.js + bảng, dữ liệu mock | `login.html` |
