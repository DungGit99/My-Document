# Stateless là gì? Stateful là gì?

## Stateless
- Là thiết kế không lưu dữ liệu của **client** trên **server**.
    - Có nghĩa là sau khi **client** gửi dữ liệu lên **server**
    - **Server** thực thi xong, trả kết quả thì quan hệ giữa **client** và **server** bị cắt đứt
    - **Server** không lưu bắt cứ dữ liệu gì của **client**

- Như vậy, Khái niệm `trạng thái` ở đây được hiểu là dữ liệu.

## Statefull

- Là 1 thiết kế ngược lại, chúng ta cần **server** để  lưu dữ liệu **client**
- Điều đó đồng nghĩa với việc ràng buộc giữa **client** và **server** vẫn giữ sau mỗi lần **request** của **client**
- **Data** được lưu lại phía **server** có thể làm **input** **parameters** cho lần kế tiếp.