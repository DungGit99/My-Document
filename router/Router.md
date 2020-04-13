# React Router là gì ?

- **React Router** là một thư viện điều hướng tiêu chuẩn trong React.
- Nó giúp cho **UI** được đồng bộ với **URL**.

## Cài đặt React Router
- **React Router** được chia ra làm nhiều package nhỏ như sau:
    - react-router
    - react-router-dom
    - react-router-native
    ```jsx
        npm install --save react-router-dom
    ```
## Chọn loại Router
- Cho những dự án web, có các loại router là `<BrowserRouter>` và `<HashRouter>`.
    - `**<BrowserRouter>**` chúng ta nên sử dụng nó khi chúng ta đã có phần code ở server để xử lý những requests động.
    - `<HashRouter>` dùng cho những trang web tĩnh.
- chúng ta luôn được khuyến khích sử dụng `<BrowserRouter>`, vì đa số dự án **React** thường đi kèm luôn với **Node.JS** để xử lý thêm nhiều vấn đề phức tạp.
- Nhưng nếu chúng ta có dự định chỉ cần trang web tĩnh là đủ thì `<HashRouter>` là giải pháp tốt hơn.

## History
-