# Life cycle của component trong ReactJS là gì?

## Life cycle gồm 3 giai đoạn
- Được tạo ra (**Mounting**)
    - có nghĩa là được add vào trong **DOM** hay có nghĩa là được hiển thị trên **UI**
- Qua nhiều thay đổi (**Updating**)
- Và bị huỷ bỏ (**Unmounting**)
    - bỏ ra khỏi **DOM** hay có nghĩa từ **UI** nó remove ra khỏi **UI**

## Mounting
- **`constructor()`**
    - Khai báo **state**
    - Định nghĩa **props** của **component**
        ```jsx
        class App() extends PureComponent {
            constructor(props) {
                super(props);
                this.DEFAULT_MAX_LENGTH = 10;

                this.state = {
                    productList: [],
                };
                }
            }
        ```
- **`render()`**
- **`componentDidMount()`**
    - Khởi tạo dữ liệu cho **component**: gọi **API**, biến đổi dữ liệu, cập nhật **state**.
    - Gửi tracking page view (GA, FacebookPixel, ...)
        ```jsx
            class HomePage extends PureComponent {

                constructor(props) {
                    super(props);
                    this.state = {
                        loading: true,
                        productList: [],
                    };
                }

                async componentDidMount() {
                    try {
                        // Send GA page view tracking
                        analytics.page('Home page');
                        const productList = await productApi.getAll();

                        this.setState({
                            productList,
                            loading: false,
                        });
                    } catch (error) {
                        console.log('Failed to fetch product list: ', error);
                        this.setState({loading: false});
                    }
                }

                render() {
                    const {loading, productList} = this.state;
                    if (loading) return <Loader />;
                    return <ProductList productList={productList}>
                }
            }
        ```

## Updating
- **`render()`**
- **`componentDidUpdate()`**
    - Cực kỳ hạn chế dùng
    - **ADVANCED** Chỉ dùng nếu muốn handle update component khi click nút back mà trên **URL** có **query** **params**.
    - Ví dụ :
        - Ở trang **Home**, đang lấy giữ liệu từ **API**, sau đó **update** vào **state**
        - Nhưng khi dữ liệu chưa được lấy xong, thì **user** đã chuyển qua trang **About**
        - Thế là **component** Home bị **umount**
        - Thì **Api** được trả về và tiếp tục gọi **setState()**
        - Nhưng mà **component** Home còn đâu mà **update**
        - Giải pháp :
            - Dùng một **flag** **isComponentMounted** để biết trạng thái của **component**.

            ```jsx
            class Home extends PureComponent {
                constructor(props) {
                    super(props);
                    this.isComponentMounted = false;
                    this.state = {
                        productList: [],
                    }
                }

            async componentDidMount() {
                this.isComponentMounted = true;
                try {
                    // bất đồng bộ mất 1, 2s ... để nó thực thi
                    // trong thời gian đó có thể unmount
                    const productList = await productApi.fetchProductList();
                    if (this.isComponentMounted) {
                        this.setState({ productList });
                    }
                } catch (error) {
                    console.log('Failed to fetch data:', error);
                }
            }

            componentWillUnmount() {
                this.isComponentMounted = false;
            }

            render() {
                // Render something here ...
            }
            ```

## Unmounting
- **`componentWillUnmount()`**
    - Cleartimeout hoặc interval nếu có dùng.
    - Reset dữ liệu trên redux nếu cần thiết.
        - Có 1 routing cha sau routing con sẽ dùng dữ liệu trên redux của routing cha
        - Sau đó vô tình đi qua routing khác routing cha, và muốn xóa dữ liệu routing cha đi nếu ko sẽ tính toán dữ liệu sai
        - Khi routing cha bị unmout thì phải reset dữ liệu trên redux
        ```jsx
            class Countdown extends PureComponent {
                constructor(props) {
                    super(props);
                    this.state = {
                        currentSecond: 0,
                    };
                }

                componentDidMount() {
                    this.timer = setInterval(() => {
                        this.setState(prevState => ({
                            currentSecond: prevState.currentSecond - 1,
                        }));
                    }, 1000);
                }

                componentWillUnmount() {
                    if (this.timer) {
                        clearInterval(this.timer);
                    }
                }

                render() {
                    const {currentSecond} = this.state;
                    return <p>{currentSecond}</p>;
                    }
                }
        ```