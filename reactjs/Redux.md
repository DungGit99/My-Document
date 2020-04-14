# Redux

- Cả 2 thư viện đều được dùng để quản lý **state** trong các ứng dụng **Javascrip**
- nó chỉ sử dụng duy nhất một **store** để lưu trữ **state**.
- thay vì sử dụng một **dispatcher**, nó dùng các **pure function** để thay đổi state.
- **Redux** bị ảnh hưởng rất nhiều bởi các nguyên tắc của **functional programming (FP)**
    ```javascript
        (state, action) => newState
    ```
- **Redux state** là **immutable**.
    - Thay vì thay đổi **state**, bạn luôn trả về một **state** mới.
    - Bạn không bao giờ trực tiếp thay đổi **object** **state** hay phụ thuộc vào tham chiếu đến **object**
    ```javascript
        // Đừng làm thế này trong Redux vì nó trực tiếp thay đổi array
        function addAuthor(state, action) {
            return state.authors.push(action.author);
        }

        // Luôn giữ state immutable và trả về object mới
        function addAuthor(state, action) {
            return [ ...state.authors, action.author ];
        }
    ```

## Store
- Trong **Redux** bạn giữ mọi **state** trong một **global** **store**/**global** **state**.
- **State object** này là nguồn dữ liệu duy nhất của bạn.
- Mặt khác, chúng ta sử dụng nhiều **reducer** để thay đổi **state** này.
- Trong **Redux** bạn chỉ cần có một **store** thôi và phản ứng với những thay đổi thông qua **reducer** và các **global** **event**.
