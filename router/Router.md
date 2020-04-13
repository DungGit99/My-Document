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
- Cho những dự án web, có các loại **router** là **<BrowserRouter>** và **<HashRouter>**.
    - **<BrowserRouter>** chúng ta nên sử dụng nó khi chúng ta đã có phần code ở **server** để xử lý những **requests** động.
    - <HashRouter> dùng cho những trang web tĩnh.
- chúng ta luôn được khuyến khích sử dụng **<BrowserRouter>**, vì đa số dự án **React** thường đi kèm luôn với **Node.JS** để xử lý thêm nhiều vấn đề phức tạp.
- Nhưng nếu chúng ta có dự định chỉ cần trang web tĩnh là đủ thì **<HashRouter>** là giải pháp tốt hơn.

## History
- Mỗi **router** đều tạo một đối tượng (**object**) **history**, được dùng để kiểm soát vị trí hiện tại của ta (**location**) và sẽ **re-render** lại website mỗi khi có sự thay đổi.
- Những component được xây dựng bởi **React Router** sẽ dựa vào **object** **history** này để **render**.
- Bất kì component nào được xây dựng bởi **React Router** mà không được bọc bởi **Router** (**<BrowserRouter>** hoặc **<HashRouter>** ) sẽ không hoạt động.

## Location
- **Router** sẽ cung cấp cho bạn một **object** **location**, nó được sử dụng rất nhiểu trong dự án. **Object** nó sẽ trông như thế này:
    ```jsx
        {
            key: 'ac3df4', // not with HashHistory!
            pathname: '/somewhere'
            search: '?some=search-string',
            hash: '#howdy',
            state: {
                [userDefined]: true
            }
        }
    ```
## Switch
- khi muốn **render** chỉ 1 **Component** trong 1 thời điểm, ta dùng thẻ tag **<Switch>**
- Nó sẽ có ý nghĩa là :
    - kiểm tra **path** có **match** hay không tuần tự từ trên xuống và ngừng lại ở **path** đầu tiên **match** với đường dẫn nhập vào.
    ```jsx
        import { Switch, Route } from 'react-router'

        <Switch>
            <Route exact path="/" component={Home}/>
            <Route path="/about" component={About}/>
            <Route path="/:user" component={User}/>
            <Route component={NoMatch}/>
        </Switch>
    ```

## Route
- Danh sách **props**
    - **path** :
        - Là một cái chuỗi dùng để định nghĩa đường dẫn mà **component** đó trỏ đến
        ```jsx
                <Route path='/trang-chu'/>
                    // Khi pathname là '/', thì component bên trong Route trên không truy cập được.
                    // Khi pathname là '/trang-chu' hoặc '/trang-chu/abc', thì truy cập được.
                    // Nếu ta muốn đường dẫn trên trỏ chính xác đến pathname '/trang-chu' thì thêm prop extact vào,
                    //lúc này truy cập vào `/trang-chu/abc' thì component bên trong Route dưới không truy cập được.
                <Route exact path='/trang-chu'/>
        ```
    - **exact**
    - **component** :
        - Truyền vào **React Component**. Khi đường dẫn trên trình duyệt đúng với **prop path** nó sẽ **render** ra **Component** này.
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
## Redirect
- Chức năng dùng để chuyển trang.
- Có thể truy xuất thông tin trang trước đó thông qua đối tượng **location**
- Để sử dụng **Redirect** ta chỉ cần **import** nó từ **react-router**.
    ```jsx
        import { Redirect } from 'react-router-dom';
    ```
- Khi bạn muốn sử dụng **location** thì tại cấu hình **Router** ta chỉ cần truyền thêm đối tượng **location** vào **component** mà cần sử dụng đối tượng **location**.
    ```jsx
       {
            path : '/login',
            exact : false,
            main : ({location}) => <Login location={location} />
        }
    ```
    ![image](https://images.viblo.asia/04995437-b55c-41ce-a740-23bdcbd10961.png)

## Links
- một cách để “di chuyển” giữa các **page**.
- **page** sẽ được chuyển tới trang cần thiết mà không cần phải load lại.
- Nó tương đương với thẻ **<a></a>**.
    ```jsx
       import { Link } from 'react-router-dom'
        const Header = () => (
            <header>
                <nav>
                    <ul>
                        <li><Link to='/'>Home</Link></li>
                        <li><Link to='/user'>User</Link></li>
                        <li><Link to='/about'>About</Link></li>
                    </ul>
                </nav>
            </header>
        )
    ```

##  Đối tượng Match
- Khi bạn muốn lấy một số thông tin ở trên **URL** thì bạn có thể dùng đối tượng **match** để lấy dữ liệu về.
    ```jsx
        {
            path : '/products',
            exact : false,
            main : ({match}) => <Products match={match} />
        }
    ```
    ![image-match](https://images.viblo.asia/f0ea77eb-f128-4e4e-bfd9-88fbaeb67f38.png)

## Đối tượng prompt - Xác nhận trước khi chuyển trang
    ```jsx
        import {Prompt} from 'react-router-dom';

        <Prompt
            when={true} // true | false
            message={ (location) => (`Ban chac chan muon di den ${location.pathname}`) }
        />
    ```
    ![image-prompt](https://images.viblo.asia/f0ea77eb-f128-4e4e-bfd9-88fbaeb67f38.png)