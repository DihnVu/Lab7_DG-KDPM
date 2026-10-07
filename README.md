![Uploading GET.png…]()
# Lab7_DG-KDPM

1. Giới thiệu về Postman
Postman là một nền tảng toàn diện cho việc phát triển, sử dụng và quản lý API. Nó cung cấp nhiều tính năng giúp các nhà phát triển và tester dễ dàng tạo, gửi, kiểm tra và chia sẻ các yêu cầu API. Postman hiện là một trong những công cụ phổ biến nhất được sử dụng trong lĩnh vực kiểm thử API.

Dưới đây là một số tính năng chính của Postman:

Tạo yêu cầu HTTP: Postman cho phép bạn tạo các yêu cầu HTTP với nhiều phương thức khác nhau (GET, POST, PUT, DELETE, v.v.). Bạn có thể nhập URL API, thêm header, body và các tham số yêu cầu.

Gửi yêu cầu và xem phản hồi: Cho phép gửi các yêu cầu HTTP đến API và xem phản hồi bao gồm: mã trạng thái HTTP (Status code), header, body và thời gian phản hồi.

Kiểm tra API: Cung cấp trình soạn thảo JSON, trình xác minh, trình gỡ lỗi và cho phép viết các script test tự động bằng JavaScript.

Chia sẻ API: Cho phép chia sẻ Collection (bộ sưu tập) và môi trường làm việc với các thành viên trong team.

Quản lý môi trường: Bạn có thể thiết lập nhiều môi trường API khác nhau (Dev, Staging, Production) để linh hoạt chuyển đổi khi test.

Tự động hóa: Hỗ trợ chạy hàng loạt các API (Collection Runner) để tự động hóa quá trình kiểm thử.

Bảo mật: Hỗ trợ đầy đủ các chuẩn xác thực như Bearer Token, OAuth 2.0, Basic Auth, v.v.

(Tại đây bạn có thể chèn một hình ảnh Giao diện Overview của Postman)

2. Kiểm thử API cơ bản
Trong phần này, chúng ta sẽ thực hành các thao tác cơ bản nhất với một API quản lý Khóa học (Course API). Giả sử chúng ta có một API gốc là [https://api.domain.com/api/v1](https://api.domain.com/api/v1).

Sử dụng biến để lưu 1 URL (Environment/Collection Variables)
Thay vì phải gõ đi gõ lại đoạn URL [https://api.domain.com/api/v1](https://api.domain.com/api/v1) cho mọi request, chúng ta sẽ lưu nó vào một biến.

Ở góc phải trên cùng của Postman, chọn Environments -> Create Environment.

Đặt tên môi trường là Dev Environment.

Thêm một biến mới:

Variable: baseUrl

Initial Value / Current Value: [https://api.domain.com/api/v1](https://api.domain.com/api/v1)

Lưu lại và chọn môi trường vừa tạo ở dropdown góc trên bên phải. Từ giờ, bạn chỉ cần gọi {{baseUrl}}.

Các yêu cầu HTTP cơ bản với API khóa học
Yêu cầu GET (Lấy dữ liệu)
Được sử dụng để lấy danh sách khóa học hoặc thông tin chi tiết của một khóa học.

Method: GET

URL: {{baseUrl}}/courses

Thao tác: Bấm Send.

Kết quả: Trả về danh sách các khóa học dưới dạng mảng JSON (Status 200 OK).

Yêu cầu POST (Tạo mới dữ liệu)
Được sử dụng để thêm một khóa học mới vào hệ thống.

Method: POST

URL: {{baseUrl}}/courses

Body: Chọn tab Body -> Chọn raw -> Chọn định dạng JSON.

JSON
{
    "name": "Khóa học Postman cơ bản",
    "description": "Hướng dẫn sử dụng Postman từ A-Z",
    "price": 500000
}
Thao tác: Bấm Send. Trả về thông tin khóa học vừa tạo (Status 201 Created).

Yêu cầu PUT (Cập nhật dữ liệu)
Được sử dụng để ghi đè/chỉnh sửa thông tin của một khóa học đã tồn tại (Ví dụ cập nhật khóa học có ID là 123).

Method: PUT

URL: {{baseUrl}}/courses/123

Body: Chọn raw -> JSON.

JSON
{
    "name": "Khóa học Postman nâng cao",
    "description": "Bổ sung thêm phần CI/CD",
    "price": 700000
}
Thao tác: Bấm Send. (Status 200 OK).

Yêu cầu DELETE (Xóa dữ liệu)
Được sử dụng để xóa một khóa học khỏi hệ thống.

Method: DELETE

URL: {{baseUrl}}/courses/123

Thao tác: Bấm Send. Khóa học sẽ bị xóa (Status 200 OK hoặc 204 No Content).

Viết Test (Kiểm tra tự động)
Để đảm bảo API hoạt động đúng, bạn chuyển sang tab Tests và viết mã JavaScript. Ví dụ: kiểm tra xem API có trả về mã 200 hay không.

JavaScript
// Kiểm tra Status Code
pm.test("Status code là 200", function () {
    pm.response.to.have.status(200);
});

// Kiểm tra thời gian phản hồi dưới 500ms
pm.test("Thời gian phản hồi < 500ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(500);
});
3. THỰC HÀNH GET của 1 API thời tiết
Để thực hành thực tế, chúng ta sẽ gọi API thời tiết công khai của OpenWeatherMap để xem nhiệt độ hiện tại của Hà Nội.

Bước 1: Lấy API Key

Truy cập [https://openweathermap.org/](https://openweathermap.org/) và tạo một tài khoản.

Vào phần My API Keys để copy chuỗi API key của bạn (ví dụ: abc123xyz...).

Bước 2: Cấu hình Request trong Postman

Method: GET

URL: [https://api.openweathermap.org/data/2.5/weather](https://api.openweathermap.org/data/2.5/weather)

Tab Params: (Nhập các thông số sau để Postman tự nối vào URL)

q = Hanoi (Tên thành phố)

units = metric (Để hiển thị độ C thay vì độ K)

appid = [API_KEY_CỦA_BẠN] (Dán key vừa copy vào đây)

Lúc này URL thực tế sinh ra sẽ là: [https://api.openweathermap.org/data/2.5/weather?q=Hanoi&units=metric&appid=abc123xyz](https://api.openweathermap.org/data/2.5/weather?q=Hanoi&units=metric&appid=abc123xyz)...

Bước 3: Gửi yêu cầu và Đọc phản hồi
Bấm nút Send. Bạn sẽ nhận được cấu trúc JSON trả về chi tiết về thời tiết Hà Nội.

JSON
{
    "weather": [
        {
            "main": "Clouds",
            "description": "overcast clouds"
        }
    ],
    "main": {
        "temp": 28.5,
        "feels_like": 30.2,
        "humidity": 75
    },
    "name": "Hanoi"
}
Ở phần phản hồi này, bạn có thể dễ dàng thấy nhiệt độ (temp) đang là 28.5°C và độ ẩm (humidity) là 75%.<img width="1917" height="972" alt="POsT" src="https://github.com/user-attachments/assets/75a5d6dc-7b2c-44b3-ba2d-544a0115272c" />
<img width="1024" height="547" alt="DuBaothoitiet" src="https://github.com/user-attachments/assets/67d9d789-27e4-4063-bb8b-583f7a995381" />
