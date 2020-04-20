# CORS là gì? Giới thiệu tất tần tật về CORS

## CORS là gì ?

- Là 1 cơ chế cho phép tài nguyên khác nhau (**fonts**, **javascript**, ...) có thể được truy vấn từ **domain** khác với **domain** của trang đó.
- Là viết tắt của từ **Cross-origin resource sharing**.

## Tại sao chúng ta cần CORS

- **CORS** được sinh ra là vì **same-origin policy**
- Là một chính sách liên quan đến bảo mật được cài đặt vào toàn bộ trình duyệt hiện nay
- Chính sách này ngăn chặn việc truy cập tài nguyên của cái **domain** vô tội vạ
- **CORS** sử dụng các HTTP header để thông báo cho trình duyệt rằng
    - một ứng dụng web chạy ở origin này ( thường là domain này ) có thể truy cập được các tài nguyên ở origin khác
- <https://topdev.vn/blog/cors-la-gi/>