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
- Cho những dự án web, có các loại router là **<BrowserRouter>** và **<HashRouter>**.
    - **<BrowserRouter>** chúng ta nên sử dụng nó khi chúng ta đã có phần code ở server để xử lý những requests động.
    - <HashRouter> dùng cho những trang web tĩnh.
- chúng ta luôn được khuyến khích sử dụng **<BrowserRouter>**, vì đa số dự án **React** thường đi kèm luôn với **Node.JS** để xử lý thêm nhiều vấn đề phức tạp.
- Nhưng nếu chúng ta có dự định chỉ cần trang web tĩnh là đủ thì **<HashRouter>** là giải pháp tốt hơn.

## History
- Mỗi **router** đều tạo một đối tượng (**object**) **history**, được dùng để kiểm soát vị trí hiện tại của ta (**location**) và sẽ **re-render** lại website mỗi khi có sự thay đổi.
- Những component được xây dựng bởi **React Router** sẽ dựa vào **object** **history** này để **render**.
- Bất kì component nào được xây dựng bởi **React Router** mà không được bọc bởi **Router** (**<BrowserRouter>** hoặc **<HashRouter>** ) sẽ không hoạt động.

## Route
- Danh sách **props**
    - **path** :
        - Là một cái chuỗi dùng để định nghĩa đường dẫn mà **component** đó trỏ đến
        ```jsx
                <Route path='/trang-chu'/>
                    // Khi pathname là '/', thì component bên trong Route trên không truy cập được.
                    // Khi pathname là '/trang-chu' hoặc '/trang-chu/abc', thì truy cập được.
                    // Nếu ta muốn đường dẫn trên trỏ chính xác đến pathname '/trang-chu' thì thêm prop extact vào, lúc này truy cập vào `/trang-chu/abc' thì component bên trong Route dưới không truy cập được.
                <Route exact path='/trang-chu'/>
        ```
    - **component** :
        - Truyền vào **React Component**. Khi đường dẫn trên trình duyệt đúng với **prop path** nó sẽ **render**ra **Component** này.
    - **render** :
        - Truyền vào một **function**, **function** này sẽ trả về React **Component**.
        - Nó cũng sẽ được gọi khi trỏ đúng đường dẫn.
        - Cái này tương tự như **prop** nhưng tiện hơn khi cần **render** một cái **component** theo dạng **inline**. Hoặc khi cần **pass** thêm những **props** khác.
    - **children**
        - Cũng là một **function** trả về một **React Component**.
        - Nhưng không giống 2 cái trên, **Component** bên trong **props** này sẽ luôn luôn được **render**, mặc cho có đúng đường dẫn dẫn hay không.
        ```jsx
            <Route path='/page' component={Page} />

            const extraProps = { color: 'red' }
            <Route path='/page' render={(props) => (<Page {...props} data={extraProps}/>)}/>
            <Route path='/page' children={(props) => (props.match ? <Page {...props}/> : <EmptyPage {...props}/> )}/>
        ```
