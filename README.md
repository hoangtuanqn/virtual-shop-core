# HỆ THỐNG MUA BÁN TÀI KHOẢN GAME VÀ DỊCH VỤ GAME

## GIỚI THIỆU TỔNG QUAN

Đây là một hệ thống website chuyên nghiệp để mua bán tài khoản game và cung cấp dịch vụ game trực tuyến. Website được xây dựng với đầy đủ các tính năng quản lý người dùng, giao dịch, thanh toán và bảo mật cao. Hệ thống hỗ trợ nhiều loại game phổ biến như Ngọc Rồng Online, Liên Quân Mobile và các game khác.

![Hình ảnh minh họa](https://i.ibb.co/R4J7Zm1n/1.png)

![Hình ảnh minh họa](https://i.ibb.co/XkL4r1T1/2.png)

![Hình ảnh minh họa](https://i.ibb.co/60GvLCdQ/2025-12-07-040601.png)

## CÔNG NGHỆ SỬ DỤNG

- **Frontend**: HTML5, CSS3, Bootstrap 4, JavaScript, jQuery
- **Backend**: PHP 7.x+
- **Database**: MySQL/MariaDB
- **Template Engine**: AdminLTE 3.x
- **UI Components**:
  - SweetAlert2 cho thông báo đẹp mắt
  - DataTables cho quản lý bảng dữ liệu
  - Chart.js cho biểu đồ thống kê
  - Select2 cho dropdown nâng cao
  - Font Awesome cho icon
  - Bootstrap DatePicker cho chọn ngày tháng

## CẤU TRÚC HỆ THỐNG

### Phân quyền người dùng

Hệ thống được chia ra 3 cấp quyền rõ ràng:

1. **User (Người dùng)**: Khách hàng thông thường
2. **Collaborator (Cộng tác viên)**: Người bán hàng/thực hiện dịch vụ
3. **Admin (Quản trị viên)**: Quản lý toàn bộ hệ thống

### Cấu trúc thư mục

```
/
├── assets/              # Tài nguyên tĩnh (JS, CSS)
├── chinhsach/           # Trang chính sách
├── controller/          # Xử lý logic nghiệp vụ
│   ├── admin/          # Controller cho admin
│   ├── client/         # Controller cho user
│   ├── partner/        # Controller cho cộng tác viên
│   └── login/          # Controller xác thực
├── core/               # Cấu hình và hàm core
│   ├── config.php     # Cấu hình database
│   └── function.php   # Các hàm dùng chung
├── cronjob/           # Tác vụ tự động
├── giaodien/          # Template và giao diện
├── lib/               # Thư viện bên thứ 3
├── pages/             # Các trang view
│   ├── admin/        # Giao diện admin
│   ├── client/       # Giao diện user
│   ├── partner/      # Giao diện cộng tác viên
│   └── view/         # Giao diện công khai
└── uploads/          # Thư mục upload file
```

## TÍNH NĂNG CHI TIẾT

### 1. HỆ THỐNG XÁC THỰC VÀ BẢO MẬT

#### 1.1 Đăng ký tài khoản

- Đăng ký với username, email, password, họ tên
- Kiểm tra độ dài username tối thiểu 6 ký tự
- Kiểm tra độ dài password tối thiểu 6 ký tự
- Xác thực định dạng email hợp lệ
- Kiểm tra username không chứa ký tự đặc biệt
- Xác nhận mật khẩu khớp
- Giới hạn số lượng tài khoản tạo từ cùng 1 IP (tối đa 10 tài khoản)
- Mật khẩu được mã hóa bằng MD5 trước khi lưu database
- Tự động tạo token và token_api cho mỗi tài khoản

#### 1.2 Đăng nhập

- Đăng nhập bằng username và password
- Chống spam login: giới hạn 5 lần nhập sai, sau đó khóa 30 giây
- Kiểm tra tài khoản có bị cấm (banned) hay không
- Lưu session/cookie để duy trì đăng nhập
- Hỗ trợ đăng nhập bằng Google OAuth
- Hỗ trợ đăng nhập bằng Zalo
- Tích hợp với Facebook (đã cấu hình fb:app_id và fb:page_id)

#### 1.3 Quên mật khẩu

- Gửi link reset mật khẩu qua email
- Có trang resetpassword.html để đặt lại mật khẩu

#### 1.4 Bảo mật đặc biệt cho Admin

- **Xác thực 2 lớp**: Khi đăng nhập vào trang quản trị, bắt buộc phải nhập mật khẩu cấp 2
- **Giới hạn số lần thử**: Chỉ được nhập sai tối đa 6 lần
- **Tự động khóa tài khoản**: Nếu nhập sai quá 6 lần, tài khoản admin sẽ bị khóa vĩnh viễn vì lý do bảo mật
- Sau mỗi lần nhập sai, số lần thử còn lại giảm dần
- Khi đăng nhập thành công, số lần thử được reset về 6
- Tạo session admin riêng biệt để phân biệt với user thường

### 2. TÍNH NĂNG MUA BÁN TÀI KHOẢN GAME

#### 2.1 Quản lý danh mục game

- Tạo nhiều danh mục game khác nhau (Ngọc Rồng, Liên Quân, etc.)
- Mỗi danh mục có thể cấu hình riêng:
  - Tên game
  - Hình ảnh đại diện
  - Mô tả chi tiết
  - Trạng thái hiển thị
  - Số lượng tài khoản đã bán (thống kê)
- Admin có thể thêm/sửa/xóa danh mục

#### 2.2 Mua bán tài khoản thông thường

- Hiển thị danh sách tài khoản theo từng danh mục
- Thông tin chi tiết mỗi tài khoản:
  - Hình ảnh tài khoản
  - Giá bán
  - Mô tả chi tiết
  - Server (máy chủ)
  - Thông tin đặc biệt của tài khoản
  - Trạng thái (còn hàng/đã bán)
- Hỗ trợ mã giảm giá khi mua:
  - Kiểm tra mã hợp lệ
  - Kiểm tra số lần sử dụng còn lại
  - Kiểm tra áp dụng cho danh mục phù hợp
  - Kiểm tra giá trị đơn hàng tối thiểu
  - Tính toán giảm giá theo phần trăm
- Quy trình mua hàng:
  - Kiểm tra số dư tài khoản
  - Trừ tiền từ ví người mua
  - Cập nhật trạng thái tài khoản đã bán
  - Ghi nhận lịch sử giao dịch
  - Cộng doanh thu cho danh mục
  - Ghi nhận biến động số dư
  - Gửi thông báo qua Telegram (nếu bật)

#### 2.3 Bán tài khoản Random (Nick ngẫu nhiên)

- Tạo nhiều bộ Random khác nhau theo danh mục
- Mỗi bộ Random có:
  - Tên bộ Random
  - Giá bán
  - Danh sách nhiều tài khoản
  - Giới hạn số lượng
- Người dùng mua sẽ nhận ngẫu nhiên 1 tài khoản trong bộ
- Hệ thống tự động chọn random và giao tài khoản
- Không thể mua lại tài khoản đã được chọn

#### 2.4 Quản lý tài khoản đã mua

- User có thể xem lịch sử tài khoản đã mua
- Xem chi tiết thông tin đăng nhập
- Xem thời gian mua
- Xem số tiền đã thanh toán
- Xem mã giảm giá đã sử dụng (nếu có)

### 3. DỊCH VỤ GAME (THUÊ DỊCH VỤ)

#### 3.1 Các loại dịch vụ hỗ trợ

Hệ thống hỗ trợ nhiều dịch vụ trong game, đặc biệt cho game Ngọc Rồng:

- Dịch vụ săn đệ tử (auto săn đệ)
- Dịch vụ cày thuê (level, nhiệm vụ)
- Dịch vụ nâng cấp trang bị
- Dịch vụ cày event
- Và các dịch vụ tùy chỉnh khác

#### 3.2 Quản lý danh mục dịch vụ

- Tạo nhiều danh mục dịch vụ khác nhau
- Mỗi danh mục có:
  - Tên dịch vụ
  - Mô tả chi tiết
  - Hình ảnh minh họa
  - Danh sách server hỗ trợ
  - Trạng thái hoạt động
  - Số đơn hàng đã thực hiện

#### 3.3 Bảng giá dịch vụ linh hoạt

- Mỗi dịch vụ có nhiều gói thuê khác nhau
- Giá khác nhau theo từng server
- Cấu hình bằng JSON format:
  ```json
  {
    "server": ["Server 1", "Server 2"],
    "gia": {
      "Server 1": 100000,
      "Server 2": 150000
    }
  }
  ```
- Admin/CTV có thể thêm/sửa/xóa gói dịch vụ

#### 3.4 Quy trình thuê dịch vụ

- Người dùng chọn dịch vụ và gói thuê
- Nhập thông tin tài khoản game:
  - Tên tài khoản
  - Mật khẩu
  - Server
- Chọn gói dịch vụ phù hợp với server
- Hỗ trợ mã giảm giá cho dịch vụ
- Kiểm tra số dư và thanh toán
- Tạo đơn hàng với trạng thái "Chờ xử lý"
- Ghi nhận biến động số dư
- Gửi thông báo Telegram cho admin

#### 3.5 Phân công đơn hàng cho Cộng tác viên

- Admin có thể gán đơn hàng cho CTV cụ thể
- CTV nhận thông báo đơn hàng mới
- CTV xem danh sách đơn hàng được giao
- Cập nhật tiến độ thực hiện
- Báo cáo hoàn thành
- Nhận hoa hồng khi hoàn thành

#### 3.6 Theo dõi trạng thái đơn hàng

Trạng thái đơn dịch vụ:

- **0**: Chờ xử lý
- **1**: Đang thực hiện
- **2**: Hoàn thành
- **3**: Hủy/Hoàn tiền

User có thể:

- Xem lịch sử đơn dịch vụ
- Xem chi tiết từng đơn
- Xem trạng thái hiện tại
- Xem người thực hiện (CTV)
- Xem thời gian thuê và hoàn thành

### 4. MUA BÁN VÀNG - NGỌC TRONG GAME

#### 4.1 Hệ thống mua vàng (Ngọc Rồng)

- Chuyển đổi tiền trong ví thành vàng game
- Cấu hình:
  - Trạng thái bật/tắt hệ thống
  - Số tiền mua tối thiểu
  - Số tiền mua tối đa
  - Danh sách server hỗ trợ
  - Trạng thái online/offline từng server
  - Tỷ giá quy đổi theo từng server
- Quy trình mua vàng:
  - Nhập số tiền muốn mua
  - Nhập tên nhân vật nhận vàng
  - Chọn server
  - Kiểm tra server online
  - Kiểm tra hạn mức tối thiểu/tối đa
  - Trừ tiền từ ví
  - Tạo đơn hàng
  - Admin/CTV xử lý và chuyển vàng vào game
- Theo dõi lịch sử mua vàng:
  - Xem số tiền đã mua
  - Số vàng nhận được
  - Trạng thái đơn hàng
  - Thời gian tạo
  - Thời gian hoàn thành

#### 4.2 Hệ thống mua ngọc (Ngọc Rồng)

- Chuyển đổi tiền trong ví thành ngọc game
- Cấu hình tương tự mua vàng:
  - Bật/tắt tính năng
  - Hạn mức tối thiểu/tối đa
  - Danh sách server
  - Trạng thái server
  - Tỷ giá quy đổi
- Quy trình mua ngọc:
  - Nhập số tiền muốn mua
  - Nhập tài khoản game
  - Nhập mật khẩu game
  - Chọn server
  - Kiểm tra điều kiện
  - Thanh toán và tạo đơn
  - Xử lý tự động/thủ công
- Tính năng đặc biệt:
  - Có thể tự động nạp ngọc vào game
  - Lưu thông tin tài khoản để xử lý
  - Bảo mật thông tin đăng nhập

#### 4.3 Hệ thống rút vàng

- Chuyển vàng trong game thành tiền mặt
- Kiểm tra số vàng trong game
- Tỷ giá quy đổi theo thị trường
- Tạo lệnh rút vàng
- Admin duyệt và chuyển tiền

#### 4.4 Hệ thống rút ngọc

- Chuyển ngọc trong game thành tiền mặt
- Cơ chế tương tự rút vàng
- Có thể rút từ nhiều server khác nhau

### 5. VÒNG QUAY MAY MẮN (MINI GAME)

#### 5.1 Tạo và quản lý vòng quay

- Admin có thể tạo nhiều vòng quay khác nhau
- Mỗi vòng quay có:
  - Tên vòng quay
  - Giá mỗi lượt quay
  - Hình ảnh vòng quay
  - 8 ô quà (từ 1 đến 8)
  - Trạng thái hoạt động

#### 5.2 Cấu hình phần quà

Mỗi ô quà trong vòng quay bao gồm:

- **Text**: Tên phần quà hiển thị
- **Item**: Giá trị phần quà
- **Tỷ lệ trúng**: Xác suất trúng phần quà (%)
- **Hình thức**: Loại phần quà
  - Tiền mặt (cộng vào ví)
  - Vàng (cộng vào item vàng)
  - Ngọc (cộng vào item ngọc)

Ví dụ cấu hình:

```
Ô 1: "10.000đ" - Tỷ lệ 30% - Tiền mặt
Ô 2: "100 vàng" - Tỷ lệ 25% - Vàng
Ô 3: "50 ngọc" - Tỷ lệ 20% - Ngọc
Ô 4: "1.000đ" - Tỷ lệ 15% - Tiền mặt
...
```

#### 5.3 Thuật toán quay ngẫu nhiên có trọng số

- Sử dụng thư viện BiasedRandom
- Tính toán xác suất dựa trên tỷ lệ được cấu hình
- Đảm bảo công bằng theo tỷ lệ đã set
- Kết quả được random theo trọng số, không phải ngẫu nhiên hoàn toàn

#### 5.4 Quy trình chơi

- User truy cập trang vòng quay
- Chọn vòng quay muốn chơi
- Kiểm tra đăng nhập
- Kiểm tra số dư (phải đủ tiền để quay)
- Click nút quay
- Hệ thống random phần quà theo tỷ lệ
- Hiển thị animation quay
- Trừ tiền lượt quay
- Cộng phần quà trúng thưởng vào tài khoản
- Lưu lịch sử quay
- Hiển thị thông báo trúng thưởng

#### 5.5 Lịch sử và thống kê

- User xem lịch sử các lần quay
- Xem phần quà đã nhận
- Admin xem thống kê:
  - Tổng số lượt quay
  - Doanh thu từ vòng quay
  - Tổng giá trị phần quà đã trao
  - Lợi nhuận

### 6. NẠP TIỀN VÀO HỆ THỐNG

#### 6.1 Nạp tiền qua thẻ cào

- Hỗ trợ nhiều loại thẻ:
  - Viettel
  - Mobifone
  - Vinaphone
  - Vietnamobile
  - Zing
  - Gate
- Mệnh giá hỗ trợ: 10k, 20k, 50k, 100k, 200k, 500k, 1.000k
- Tích hợp API nạp thẻ tự động
- Quy trình nạp thẻ:
  - Chọn loại thẻ
  - Chọn mệnh giá
  - Nhập số seri
  - Nhập mã thẻ
  - Gửi lên hệ thống
  - Chờ API xử lý (trạng thái "xuly")
  - Nhận kết quả:
    - **hoantat**: Nạp thành công - Cộng tiền vào ví
    - **thatbai**: Nạp thất bại - Không cộng tiền
- Giới hạn chống spam:
  - Tối đa 3 thẻ chờ duyệt cùng lúc
  - Nếu nạp sai nhiều thẻ (6 thẻ thất bại/ngày), tạm khóa nạp thẻ trong ngày
- Chiết khấu thẻ:
  - Admin cấu hình % chiết khấu
  - Số tiền nhận = Mệnh giá × (100% - Chiết khấu%)
- Lưu lịch sử nạp thẻ đầy đủ

#### 6.2 Nạp tiền qua ngân hàng

- Tích hợp API ngân hàng tự động
- Hỗ trợ nhiều ngân hàng:
  - MB Bank
  - Vietcombank
  - Techcombank
  - Và các ngân hàng khác
- Quy trình nạp:
  - Hệ thống tạo nội dung chuyển khoản dạng: "NAPTIEN[ID]"
  - User chuyển khoản theo cú pháp đúng
  - Cronjob tự động quét giao dịch ngân hàng mỗi phút
  - Đối chiếu nội dung chuyển khoản với ID user
  - Tự động cộng tiền vào tài khoản
  - Ghi nhận lịch sử giao dịch
  - Gửi thông báo Telegram
- Tính năng nổi bật:
  - Nạp 24/7 tự động
  - Không cần admin duyệt
  - Cộng tiền ngay lập tức
  - Chiết khấu ATM có thể cấu hình
  - Tránh trùng lặp giao dịch (kiểm tra mã giao dịch)

#### 6.3 Cronjob tự động xử lý

File: `/cronjob/Bank.php`

- Chạy: Mỗi 1 phút
- Nhiệm vụ:
  - Gọi API ngân hàng
  - Lấy danh sách giao dịch mới
  - Parse nội dung chuyển khoản
  - Cộng tiền tự động
  - Gửi thông báo

### 7. QUẢN LÝ MÃ GIẢM GIÁ (COUPON)

#### 7.1 Tạo mã giảm giá

Admin/CTV có thể tạo coupon với các thông tin:

- **Code**: Mã giảm giá (ví dụ: NOEL2024)
- **Phần trăm giảm**: 10%, 20%, 50%, etc.
- **Áp dụng cho**:
  - Mua tài khoản thường
  - Mua tài khoản random
  - Thuê dịch vụ
  - Mua thẻ
- **Số lượng**: Giới hạn số lần sử dụng
- **Số tiền tối thiểu**: Đơn hàng tối thiểu để áp dụng
- **Loại mã**:
  - Sử dụng 1 lần/user
  - Sử dụng nhiều lần
- **Trạng thái**: Bật/Tắt

#### 7.2 Sử dụng mã giảm giá

- User nhập mã khi thanh toán
- Hệ thống kiểm tra:
  - Mã có tồn tại không
  - Còn lượt sử dụng không
  - Áp dụng đúng danh mục không
  - Đơn hàng đủ giá trị tối thiểu chưa
  - User đã dùng mã này chưa (nếu mã dùng 1 lần)
- Nếu hợp lệ:
  - Tính số tiền giảm = Giá × Phần trăm giảm
  - Trừ số lượng mã
  - Ghi nhận mã đã sử dụng trong đơn hàng

#### 7.3 Quản lý và thống kê

- Xem danh sách tất cả mã
- Xem số lượng đã dùng/còn lại
- Xem doanh thu từ đơn có mã giảm giá
- Sửa/Xóa mã giảm giá
- Tìm kiếm mã theo code

### 8. HỆ THỐNG QUẢN TRỊ (ADMIN)

#### 8.1 Dashboard tổng quan

- Thống kê tổng quan:
  - Tổng doanh thu hôm nay
  - Tổng doanh thu tháng này
  - Số lượng user mới
  - Số đơn hàng đang xử lý
  - Số tài khoản đang bán
  - Số dịch vụ đang thực hiện
- Biểu đồ:
  - Doanh thu theo ngày
  - Số đơn hàng theo tháng
  - Tỷ lệ loại dịch vụ
- Top nạp thẻ trong tháng
- Hoạt động gần đây

#### 8.2 Quản lý người dùng

- Xem danh sách tất cả user
- Tìm kiếm user theo username, email
- Thông tin chi tiết user:
  - Thông tin cá nhân
  - Số dư tài khoản
  - Tổng nạp
  - Tổng chi
  - Item vàng
  - Item ngọc
  - Trạng thái banned
  - IP đăng ký
  - Thời gian đăng ký
- Chỉnh sửa thông tin user:
  - Đổi mật khẩu
  - Cộng/Trừ tiền
  - Cộng/Trừ vàng, ngọc
  - Đổi quyền (user/ctv/admin)
  - Banned/Unban
- Xem lịch sử giao dịch của user:
  - Lịch sử nạp tiền
  - Lịch sử mua hàng
  - Lịch sử thuê dịch vụ
  - Biến động số dư
- Gửi thông báo cho user

#### 8.3 Quản lý tài khoản game

- Thêm tài khoản game mới:
  - Chọn danh mục game
  - Loại tài khoản (thường/random)
  - Giá bán
  - Server
  - Thông tin đăng nhập
  - Mô tả chi tiết
  - Upload hình ảnh
  - Trạng thái hiển thị
- Sửa thông tin tài khoản
- Xóa tài khoản
- Xem tài khoản đã bán
- Thống kê tài khoản theo:
  - Danh mục
  - Server
  - Giá
  - Tình trạng bán

#### 8.4 Quản lý đơn hàng

- **Đơn mua tài khoản**:

  - Xem chi tiết đơn hàng
  - Thông tin buyer
  - Tài khoản đã mua
  - Số tiền thanh toán
  - Mã giảm giá (nếu có)
  - Thời gian mua
  - Hoàn tiền nếu có vấn đề

- **Đơn thuê dịch vụ**:

  - Danh sách đơn chờ xử lý
  - Danh sách đơn đang làm
  - Danh sách đơn hoàn thành
  - Gán đơn cho CTV
  - Cập nhật trạng thái đơn
  - Xác nhận hoàn thành
  - Hoàn tiền/Hủy đơn

- **Đơn mua vàng/ngọc**:

  - Xem danh sách đơn
  - Chi tiết: user, server, số tiền, số vàng/ngọc
  - Cập nhật trạng thái xử lý
  - Xác nhận đã chuyển

- **Đơn rút vàng/ngọc**:
  - Kiểm tra yêu cầu rút
  - Xác minh số dư trong game
  - Duyệt và chuyển tiền
  - Từ chối với lý do

#### 8.5 Quản lý thanh toán

- **Quản lý nạp thẻ**:

  - Xem lịch sử nạp thẻ
  - Trạng thái thẻ: Chờ xử lý/Thành công/Thất bại
  - Duyệt thẻ thủ công (nếu cần)
  - Xem thống kê theo loại thẻ
  - Cấu hình chiết khấu thẻ

- **Quản lý nạp ATM**:

  - Xem lịch sử giao dịch ngân hàng
  - Thông tin: User, số tiền, mã GD, nội dung
  - Kiểm tra giao dịch bị lỗi
  - Xử lý thủ công nếu cần

- **Cấu hình ngân hàng**:
  - Thêm/Sửa thông tin tài khoản ngân hàng
  - Bật/Tắt nạp tự động
  - Cấu hình API key
  - Cấu hình nội dung nạp tiền
  - Chiết khấu ATM

#### 8.6 Quản lý dịch vụ

- Thêm danh mục dịch vụ mới
- Cấu hình server hỗ trợ
- Thêm/Sửa gói dịch vụ
- Cấu hình giá theo server
- Bật/Tắt dịch vụ
- Xem thống kê đơn dịch vụ

#### 8.7 Quản lý vòng quay

- Tạo vòng quay mới
- Cấu hình 8 phần quà
- Set tỷ lệ trúng từng phần quà
- Cấu hình giá mỗi lượt
- Upload hình ảnh vòng quay
- Bật/Tắt vòng quay
- Xem thống kê:
  - Tổng lượt quay
  - Doanh thu
  - Giá trị phần quà đã trao
  - Lợi nhuận

#### 8.8 Quản lý cộng tác viên

- Xem danh sách CTV
- Set quyền CTV cho user
- Xem doanh thu của từng CTV
- Quản lý hoa hồng:
  - Cấu hình % hoa hồng mặc định
  - Set % hoa hồng cho từng CTV
  - Xem tổng hoa hồng đã trả
- Duyệt lệnh rút tiền của CTV
- Xóa quyền CTV

#### 8.9 Cấu hình hệ thống

- **Thông tin website**:

  - Tên website
  - Logo
  - Favicon
  - Mô tả SEO
  - Keywords
  - Thông tin liên hệ
  - Social links

- **Cấu hình chức năng**:

  - Bật/Tắt đăng ký
  - Bật/Tắt nạp thẻ
  - Bật/Tắt nạp ATM
  - Bật/Tắt mua vàng
  - Bật/Tắt mua ngọc
  - Bật/Tắt vòng quay
  - Bật/Tắt thuê dịch vụ

- **Cấu hình Telegram**:

  - Bot Token
  - Chat ID
  - Bật/Tắt thông báo
  - Test thông báo

- **Cấu hình bảo mật**:

  - Chặn IP
  - Danh sách IP đen
  - Giới hạn tạo tài khoản/IP
  - Bảo trì website

- **Cấu hình Top nạp**:
  - Bật/Tắt bảng xếp hạng
  - Giải thưởng top 1, 2, 3
  - Thời gian reset top (hàng tháng)
  - Cronjob reset tự động

#### 8.10 Quản lý IP đen

- Thêm IP vào blacklist
- Xóa IP khỏi blacklist
- Xem lý do chặn
- Thời gian chặn
- Người thêm vào blacklist

### 9. HỆ THỐNG CỘNG TÁC VIÊN (PARTNER/CTV)

#### 9.1 Dashboard CTV

- Thống kê riêng của CTV:
  - Tổng đơn hàng thành công
  - Tổng hoa hồng đã nhận
  - Số tài khoản đang bán
  - Số dịch vụ đang thực hiện
  - Doanh thu trong tháng
  - Số dư khả dụng
- Biểu đồ doanh thu cá nhân
- Thông báo đơn hàng mới

#### 9.2 Quản lý tài khoản của CTV

- CTV có thể thêm tài khoản game để bán:
  - Chọn danh mục
  - Nhập thông tin tài khoản
  - Đặt giá bán
  - Upload hình ảnh
  - Gửi duyệt hoặc đăng trực tiếp (tùy cấu hình)
- Xem tài khoản đang bán
- Sửa thông tin tài khoản
- Xóa tài khoản chưa bán
- Xem tài khoản đã bán
- Nhận hoa hồng khi bán được

#### 9.3 Quản lý Random của CTV

- Tạo bộ Random riêng
- Thêm nhiều tài khoản vào bộ Random
- Đặt giá cho bộ Random
- Xem số tài khoản đã bán trong bộ
- Nhận hoa hồng từ Random đã bán

#### 9.4 Nhận và xử lý đơn dịch vụ

- **Đơn dịch vụ được phân công**:

  - Admin gán đơn cho CTV
  - CTV nhận thông báo
  - Xem chi tiết đơn hàng
  - Thông tin tài khoản khách
  - Yêu cầu dịch vụ
  - Hạn hoàn thành

- **Đơn dịch vụ đang làm**:

  - Xem danh sách đơn đang thực hiện
  - Cập nhật tiến độ
  - Ghi chú quá trình thực hiện
  - Báo cáo vấn đề (nếu có)

- **Hoàn thành đơn hàng**:
  - Đánh dấu hoàn thành
  - Gửi kết quả cho admin duyệt
  - Admin xác nhận
  - Nhận hoa hồng vào ví

#### 9.5 Quản lý dịch vụ

- CTV có thể tạo gói dịch vụ riêng (nếu được phép)
- Cấu hình giá dịch vụ
- Chọn server hỗ trợ
- Bật/Tắt dịch vụ của mình

#### 9.6 Quản lý mã giảm giá

- CTV có thể tạo mã giảm giá riêng
- Áp dụng cho sản phẩm/dịch vụ của mình
- Xem thống kê mã giảm giá
- Số lần sử dụng
- Doanh thu từ mã giảm giá

#### 9.7 Lịch sử và thống kê

- **Lịch sử bán hàng**:

  - Tài khoản đã bán
  - Số tiền thu được
  - Hoa hồng nhận được
  - Thời gian bán

- **Lịch sử dịch vụ**:

  - Đơn dịch vụ đã làm
  - Trạng thái đơn
  - Hoa hồng nhận được
  - Đánh giá từ khách (nếu có)

- **Biến động số dư**:
  - Xem chi tiết các khoản thu nhập
  - Nguồn thu: Bán tài khoản, dịch vụ, hoa hồng
  - Các khoản chi: Rút tiền

#### 9.8 Rút tiền CTV

- **Tạo lệnh rút tiền**:

  - Nhập số tiền muốn rút
  - Chọn ngân hàng
  - Nhập số tài khoản
  - Nhập tên chủ tài khoản
  - Gửi yêu cầu

- **Điều kiện rút tiền**:

  - Số dư phải lớn hơn số tiền rút
  - Có thể có hạn mức rút tối thiểu
  - Đã xác thực tài khoản

- **Xử lý lệnh rút**:

  - Trạng thái: Chờ duyệt
  - Admin kiểm tra và duyệt
  - Chuyển khoản cho CTV
  - Cập nhật trạng thái: Đã thanh toán
  - Trừ tiền trong ví CTV

- **Lịch sử rút tiền**:
  - Xem các lệnh rút
  - Trạng thái từng lệnh
  - Số tiền đã rút
  - Thời gian xử lý

#### 9.9 Thống kê doanh thu

- Doanh thu theo ngày/tháng/năm
- Biểu đồ xu hướng
- So sánh các kỳ
- Top sản phẩm bán chạy
- Top dịch vụ được thuê nhiều

### 10. TÍNH NĂNG USER (KHÁCH HÀNG)

#### 10.1 Trang chủ

- Hiển thị danh mục game
- Banner quảng cáo
- Tài khoản nổi bật
- Dịch vụ hot
- Vòng quay may mắn
- Top nạp thẻ
- Tin tức/Sự kiện

#### 10.2 Trang cá nhân

- Thông tin tài khoản:
  - Username
  - Email
  - Họ tên
  - Số điện thoại
  - Địa chỉ
  - Số dư
  - Item vàng
  - Item ngọc
  - Tổng nạp
  - Ngày đăng ký
- Đổi thông tin cá nhân
- Đổi mật khẩu
- Đổi mật khẩu cấp 2 (nếu là admin)

#### 10.3 Lịch sử giao dịch

- **Lịch sử nạp tiền**:

  - Nạp thẻ cào
  - Nạp ATM
  - Trạng thái, số tiền, thời gian

- **Lịch sử mua tài khoản**:

  - Tài khoản đã mua
  - Thông tin đăng nhập
  - Giá mua
  - Mã giảm giá
  - Thời gian mua

- **Lịch sử thuê dịch vụ**:

  - Dịch vụ đã thuê
  - Trạng thái
  - Người thực hiện
  - Thời gian

- **Lịch sử mua vàng/ngọc**:

  - Đơn hàng
  - Số lượng
  - Trạng thái

- **Lịch sử vòng quay**:

  - Số lần quay
  - Phần quà nhận được

- **Biến động số dư**:
  - Số dư trước/sau
  - Lý do thay đổi
  - Thời gian

#### 10.4 Mua sắm

- Duyệt danh mục
- Lọc theo:
  - Giá
  - Server
  - Thuộc tính
- Sắp xếp
- Xem chi tiết sản phẩm
- Thêm mã giảm giá
- Thanh toán

#### 10.5 Liên hệ và hỗ trợ

- Form liên hệ
- Thông tin liên hệ
- Link mạng xã hội
- FAQ

### 11. TÍNH NĂNG BẢO MẬT NÂNG CAO

#### 11.1 Mã hóa mật khẩu

- Sử dụng MD5 để hash password
- Không lưu password dạng plain text
- Mật khẩu cấp 2 cho admin được mã hóa riêng

#### 11.2 Chống tấn công

- **Chống Brute Force Login**:

  - Giới hạn số lần đăng nhập sai
  - Khóa tạm thời sau nhiều lần sai
  - Theo dõi IP đăng nhập

- **Chống SQL Injection**:

  - Sử dụng prepared statements
  - Escape input data
  - Validate dữ liệu đầu vào

- **Chống XSS**:

  - Sử dụng htmlspecialchars()
  - Filter input, escape output
  - Content Security Policy

- **Chống CSRF**:
  - Token validation
  - Check HTTP method
  - Verify origin

#### 11.3 Quản lý session

- Session timeout
- Regenerate session ID
- Secure cookie flags
- HttpOnly cookies

#### 11.4 IP Blacklist

- Chặn IP độc hại
- Tự động chặn IP spam
- Whitelist cho IP admin
- Log truy cập bất thường

#### 11.5 Giới hạn và Rate Limiting

- Giới hạn số lần đăng ký từ 1 IP
- Giới hạn số lần nạp thẻ/ngày
- Giới hạn số lần gọi API
- Throttling các hành động nhạy cảm

### 12. TÍCH HỢP VÀ API

#### 12.1 Tích hợp thanh toán

- API nạp thẻ cào (card.tuanori.vn hoặc tương tự)
- API ngân hàng tự động
- Webhook callback xử lý giao dịch

#### 12.2 Tích hợp Telegram Bot

- Gửi thông báo đơn hàng mới
- Thông báo nạp tiền thành công
- Thông báo tài khoản đã bán
- Thông báo dịch vụ hoàn thành
- Báo cáo thống kê hàng ngày

#### 12.3 OAuth Social Login

- Google OAuth 2.0
  - Đăng nhập nhanh bằng Google
  - Lấy thông tin email, tên
  - Tạo tài khoản tự động
- Facebook Login (đã cấu hình)
- Zalo Login

#### 12.4 Email Service

- Gửi email xác thực
- Gửi email reset password
- Gửi email thông báo giao dịch
- SMTP configuration

### 13. CRONJOB TỰ ĐỘNG

#### 13.1 Cronjob xử lý ngân hàng

File: `/cronjob/Bank.php`

- Chạy: Mỗi 1 phút
- Nhiệm vụ:
  - Gọi API ngân hàng
  - Lấy danh sách giao dịch mới
  - Parse nội dung chuyển khoản
  - Cộng tiền tự động
  - Gửi thông báo

#### 13.2 Cronjob xử lý Momo

File: `/cronjob/Momo.php`

- Chạy: Mỗi 1 phút
- Nhiệm vụ:
  - Kiểm tra giao dịch Momo
  - Xử lý nạp tiền qua Momo
  - Tương tự Bank

#### 13.3 Cronjob reset Top nạp

File: `/cronjob/ResetTopNap.php`

- Chạy: Đầu tháng (ngày 1 hàng tháng)
- Nhiệm vụ:
  - Lưu top nạp tháng trước
  - Trao thưởng cho top 1, 2, 3
  - Reset total_topnapthe về 0
  - Bắt đầu tháng mới

### 14. RESPONSIVE VÀ UX/UI

#### 14.1 Responsive Design

- Tương thích mọi thiết bị:
  - Desktop
  - Tablet
  - Mobile
- Bootstrap 4 responsive grid
- Mobile-first approach
- Touch-friendly interface

#### 14.2 UI/UX

- Giao diện đẹp mắt, chuyên nghiệp
- Màu sắc hài hòa
- Loading animation
- Smooth transitions
- Toast notifications (SweetAlert2)
- Modal popups
- Datatable với search, sort, pagination

#### 14.3 Performance

- Minified CSS/JS
- Image optimization
- Lazy loading
- CDN cho thư viện
- Caching
- Optimized queries

### 15. TÍNH NĂNG BỔ SUNG

#### 15.1 Chính sách và điều khoản

- Trang chính sách (folder `/chinhsach/`)
- Điều khoản sử dụng
- Chính sách bảo mật
- Hướng dẫn sử dụng
- Quy định giao dịch

#### 15.2 SEO-Friendly

- Meta tags đầy đủ
- Open Graph tags (Facebook)
- Canonical URLs
- Sitemap
- robots.txt
- Structured data

#### 15.3 Analytics

- Google Analytics tích hợp
- Tracking pageviews
- Tracking conversions
- Tracking events
- User behavior analysis

#### 15.4 Chống SPAM và BOT

- Anti-DevTools script
- Disable right-click (tùy chọn)
- Captcha (có thể tích hợp)
- Rate limiting
- Bot detection

#### 15.5 Nhập nick từ file

- Upload file Excel/CSV
- Import hàng loạt tài khoản
- Validate dữ liệu
- Báo lỗi chi tiết
- Xem kết quả import

#### 15.6 Mua thẻ game

- Hệ thống mua thẻ game tự động
- Nhiều loại thẻ: Garena, Zing, Gate, Vcoin...
- Nhiều mệnh giá
- Mã thẻ tự động gửi về
- Lịch sử mua thẻ

## HƯỚNG DẪN CÀI ĐẶT

### Yêu cầu hệ thống

- PHP >= 7.2
- MySQL/MariaDB >= 5.7
- Apache/Nginx web server
- mod_rewrite enabled
- cURL extension
- GD Library (cho xử lý ảnh)
- Cronjob support

### Các bước cài đặt

1. **Clone hoặc download source code**
2. **Cấu hình database**

   - Tạo database mới
   - Import file `tuanori.sql`
   - Chỉnh sửa `/core/config.php`:
     ```php
     $this->ketnoi = mysqli_connect('localhost', 'username', 'password', 'database_name');
     ```

3. **Cấu hình base URL**

   - Mở `/core/config.php`
   - Thay đổi: `$base_url = 'https://your-domain.com/';`

4. **Phân quyền thư mục**

   ```bash
   chmod 755 /uploads/
   chmod 644 /core/config.php
   ```

5. **Cấu hình cronjob**

   ```bash
   * * * * * php /path/to/cronjob/Bank.php
   * * * * * php /path/to/cronjob/Momo.php
   0 0 1 * * php /path/to/cronjob/ResetTopNap.php
   ```

6. **Cấu hình API**

   - API nạp thẻ: Đăng ký tại đối tác thẻ cào
   - API ngân hàng: Cấu hình trong admin panel
   - Telegram Bot: Tạo bot và lấy token

7. **Đăng nhập admin**

   - URL: `your-domain.com/admin`
   - Username/Password: Xem trong database table `users` (user có level = 'admin')

8. **Cấu hình hệ thống**
   - Vào Admin > Setting
   - Điền đầy đủ thông tin website
   - Cấu hình các tính năng
   - Cấu hình thanh toán
   - Test các chức năng

## BẢO MẬT

- Thay đổi thông tin database trong `/core/config.php` ngay sau khi cài đặt
- Đổi mật khẩu admin mặc định
- Đặt mật khẩu cấp 2 phức tạp
- Không share thông tin admin
- Backup database định kỳ
- Update security patches thường xuyên
- Monitor log files
- Bật HTTPS (SSL Certificate)

## HỖ TRỢ VÀ LIÊN HỆ

- Developer: TUANORI
- Zalo: 0812665001
- Copyright: Powered by FB @TranDucAnh.Profile

## GIẤY PHÉP

Mã nguồn này được phát triển bởi TUANORI. Vui lòng liên hệ để biết thông tin về giấy phép sử dụng.

## GHI CHÚ QUAN TRỌNG

- Đây là hệ thống thương mại điện tử, cần tuân thủ pháp luật về thương mại điện tử
- Cần có giấy phép kinh doanh hợp lệ
- Tuân thủ quy định về bảo vệ thông tin cá nhân
- Không sử dụng cho mục đích trái pháp luật
- Developer không chịu trách nhiệm về việc sử dụng sai mục đích

---

_README này được tạo tự động dựa trên phân tích mã nguồn. Vui lòng liên hệ developer để biết thêm chi tiết._
